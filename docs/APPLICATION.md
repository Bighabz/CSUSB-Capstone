# Set up the capstone application

The [main README](../README.md) explains the project and its features. This guide covers the database, local configuration, and the included Windows/IIS deployment.

## Configure the application

Create a virtual environment and install `requirements.txt`, then copy `.env.example` to the ignored `.env` file. Use your own values:

| Setting | What to enter |
| --- | --- |
| `FLASK_ENV` | `development` for local HTTP testing; leave it empty for deployment. |
| `FLASK_SECRET_KEY` | A new random value for session signing. |
| `DB_ENGINE` | `mysql` or `mssql`. |
| `DB_HOST`, `DB_PORT` | Your database host and port. `localhost` is the local example. |
| `DB_NAME`, `DB_USER`, `DB_PASSWORD` | The demo database and its application user's credentials. |
| `UNIVERSITY_AI_API_URL`, `UNIVERSITY_AI_API_KEY`, `UNIVERSITY_AI_MODEL` | Your university or compatible AI provider settings. |
| `ADMIN_BASIC_USER`, `ADMIN_BASIC_PASS` | Credentials for the restricted submissions endpoint. |
| `AI_BUDGET_DAILY`, `AI_BUDGET_HOURLY` | Caps on AI requests for this application process. |

Generate the session key locally with `python -c "import secrets; print(secrets.token_hex(32))"`, then put it in `.env`. Keep the file out of Git.

## Prepare the database

Use a new demo database. The schema files drop and recreate the application's tables, so do not run them against data you need to keep.

For MySQL/MariaDB, sign in with a database administrator in the MySQL client and run:

```sql
SOURCE database/schema_mysql.sql;
```

Run this from the repository root, or give `SOURCE` the full path to the schema file. It creates the `ironbark` database and loads fictional products, services, and engagements.

Create a separate application user. For a local MySQL installation:

```sql
CREATE USER 'ironbark_app'@'localhost' IDENTIFIED BY 'choose-a-new-password';
GRANT SELECT, INSERT, UPDATE ON ironbark.* TO 'ironbark_app'@'localhost';
```

For separate web and database servers, replace the host restriction with your web server's actual host and allow database traffic only from that server. Enter the application credentials in `.env`; the web app should not use the administrator login.

For SQL Server, install an appropriate Microsoft ODBC driver and `pyodbc`, run `database/schema_mssql.sql` in SQL Server Management Studio, and create a restricted application login. Set `DB_ENGINE=mssql` and `DB_ODBC_DRIVER` to the installed driver's name.

## Start and check the site

Run `python app.py` and open **http://localhost:8000**.

- The catalog should load products from the database and respond to search/filter changes.
- The contact form should save an inquiry.
- On `/status`, use `IB-2026-0042` with `cto@northridge-mfg.example`. Both are fictional seed data.
- With the AI provider configured, try ClaWD, a catalog recommendation, and an engagement summary.

If local forms fail, confirm that `FLASK_ENV=development` is set before starting the app. Secure cookies require HTTPS when that development setting is absent. Database errors usually mean the schema, user privileges, driver, or connection values need attention.

## Deploy with Windows Server and IIS

The included `web.config` starts Waitress through IIS HttpPlatformHandler. Its example application directory is `C:\inetpub\ironbark`, with a Python environment named `venv`.

1. Install Python, IIS with its CGI feature, and [HttpPlatformHandler](https://www.iis.net/downloads/microsoft/httpplatformhandler).
2. Copy the project into your chosen application directory and create a `logs` subdirectory.
3. Create the environment and install dependencies:

```powershell
cd C:\inetpub\ironbark
python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
```

4. Fill in `.env` with the deployment's credentials. Leave `FLASK_ENV` empty and configure HTTPS in IIS.
5. In IIS Manager, create a site pointing at the application directory. Match its binding to your chosen hostname.
6. Give the application pool identity read access to the project and write access only to its log directory. Restrict access to `.env`.
7. Check `web.config` paths if you used a different directory or environment name.
8. Open the site and repeat the checks above. Review the private stdout logs if startup fails.

The example uses one Waitress process. Request limits and AI budgets are kept in process memory; deployments with multiple processes need a shared store for those limits.

## Before making a deployment public

Use HTTPS, new credentials, a restricted database user, and a database firewall rule limited to the web server. Confirm that requests for `.env`, `.git`, source files, and logs are blocked. Check an unauthenticated request to the admin endpoint and verify that it is refused.

These are deployment checks, not a claim that the coursework application has passed a production penetration test.
