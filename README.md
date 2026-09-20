# CSUSB Capstone

**IST 4910 · Spring 2026 · California State University, San Bernardino**

The DOGPARK team's capstone combines an enterprise IT/security environment with **IronBark Security Solutions**, a fictional cybersecurity company's full-stack web application.

**Team:** Habib Jahshan, Liam Pearson, Brandon Deane, and Danny Hernandez.

## Project scope

- **Application:** Flask website with a searchable, database-backed product and service catalog, contact forms, and engagement-status lookup.
- **AI integration:** ClaWD customer service chatbot, contextual catalog recommendations, and engagement summaries through the CSUSB University AI API. API credentials remain on the server.
- **Infrastructure:** Windows Server/IIS web deployment and a separate database VM, with MySQL/MariaDB and Microsoft SQL Server schema options.
- **Security:** parameterized database queries, CSRF protection, request limits, security headers, and IIS filtering for sensitive files.
- **Project evidence:** application, infrastructure, audit, and penetration-testing screenshots collected alongside the source code.

## Explore the project

| Area | Files |
| --- | --- |
| Web application and AI proxy | [app.py](app.py), [templates/](templates/), [static/](static/) |
| Database layer and schemas | [database/](database/) |
| IIS deployment configuration | [web.config](web.config) |
| Setup and deployment guide | [docs/APPLICATION.md](docs/APPLICATION.md) |
| Product requirements | [docs/PRD.md](docs/PRD.md) |
| Environment template | [.env.example](.env.example) |

## Screenshots

### Application dashboard
![IronBark application dashboard](dashboard.png)

### Infrastructure
![Capstone infrastructure](infra.png)

### AI customer service
![ClaWD AI customer service](clawd.png)

Additional evidence: [audit](audit.png), [penetration testing](pentest.png), and [SecurePack](securepack.png).

## Run locally

```bash
git clone https://github.com/Bighabz/CSUSB-Capstone.git
cd CSUSB-Capstone
python -m venv .venv
source .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
pip install PyMySQL       # or pyodbc for Microsoft SQL Server
cp .env.example .env
```

Configure your database and university AI API settings in `.env`, initialize the appropriate schema from `database/`, then run `python app.py`. Full database, IIS, and verification instructions are in the [application guide](docs/APPLICATION.md).

## Repository history

This repository consolidates the former **IronBark-app** application repository and **IronBark** project-evidence repository. Both Git histories are preserved in the consolidation merge. IronBark remains the fictional company name used inside the coursework application.
