# CSUSB Capstone

For our Spring 2026 capstone at California State University, San Bernardino, our team built a website for **IronBark Security Solutions**, a fictional cybersecurity company. The project brings together a customer-facing application, a database, AI-assisted support, and a Windows server deployment.

**Course:** IST 4910 · **Team DOGPARK:** Habib Jahshan, Liam Pearson, Brandon Deane, and Danny Hernandez.

## What a visitor can do

| Feature | What it does |
| --- | --- |
| **Browse services and products** | Search and filter a catalog backed by a SQL database. |
| **Ask for a recommendation** | Describe a business need and get an AI-assisted explanation of relevant catalog options. |
| **Chat with ClaWD** | Ask the customer service assistant questions from any page. |
| **Check project status** | Look up a sample engagement with its code and email, then request a plain-language summary. |
| **Contact the company** | Submit an inquiry through a form that saves it to the database. |

The company, catalog, and sample customer records are coursework examples. The illustrations included in the repository are branding assets.

## What we built behind it

The site uses **Python and Flask**, with **MySQL/MariaDB or Microsoft SQL Server** for its data. The deployment files support **Windows Server and IIS**. AI requests go through the backend, so visitors do not receive the API key.

The application includes checks against forged form submissions, limits on repeated requests, parameterized database queries, and browser security headers. The IIS configuration blocks direct access to source files and local settings. Server permissions, database credentials, and HTTPS still need to be configured for each deployment.

## Run it locally

You need Python 3.11 or later and a MySQL/MariaDB database. SQL Server is also supported; see the [setup guide](docs/APPLICATION.md).

```bash
git clone https://github.com/Bighabz/CSUSB-Capstone.git
cd CSUSB-Capstone
python -m venv .venv
```

Activate `.venv` with `source .venv/bin/activate` on macOS/Linux or `.\.venv\Scripts\Activate.ps1` in Windows PowerShell. Then:

```bash
python -m pip install -r requirements.txt
python -c "from pathlib import Path; import shutil; p=Path('.env'); shutil.copyfile('.env.example',p) if not p.exists() else None"
```

1. Edit `.env`: set `FLASK_ENV=development`, a new `FLASK_SECRET_KEY`, your database connection, and your AI provider settings.
2. Use the [database setup instructions](docs/APPLICATION.md#prepare-the-database) to create a disposable demo database and application user. The seed scripts recreate the demo tables.
3. Start the site:

```bash
python app.py
```

Open **http://localhost:8000**. Browse the catalog and try the sample status lookup listed in the setup guide. AI features require a valid university or compatible provider key in your local `.env`.

## Explore the source

| Area | Where to look |
| --- | --- |
| Pages and application routes | [app.py](app.py), [templates/](templates/), [static/](static/) |
| Database connection and sample data | [database/](database/) |
| Local setup and IIS deployment | [docs/APPLICATION.md](docs/APPLICATION.md), [web.config](web.config) |
| Course requirements | [docs/PRD.md](docs/PRD.md) |
| Configuration template | [.env.example](.env.example) |

This repository combines the former IronBark-app source and IronBark assets, with their development histories brought together. **CSUSB Capstone** is the project name; **IronBark** remains the fictional business used in the application.
