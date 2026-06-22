# Frappe Bench Monorepo

This repository contains a Frappe Bench installation with multiple apps and tooling for local development, testing, and deployment.

Contents
- apps/: Contains Frappe apps such as `frappe`, `erpnext`, `frappe_whatsapp`, `builder`, and others.
- config/: Configuration files for Redis and scheduler processes.
- env/: A Python virtual environment used for development.
- logs/: Runtime logs.
- sites/: Multi-site configuration and assets.

Quick overview

This workspace appears to be a development snapshot of a Frappe/ERPNext bench. Key apps include:

- `frappe/` — The Frappe framework source and utilities.
- `erpnext/` — ERPNext application built on Frappe.
- `builder/` — Tooling and frontends for building app assets.
- `frappe_whatsapp` and `frappe_whatsapp_chatbot` — WhatsApp integration apps.

Prerequisites

- macOS or Linux (development tested here).
- Python 3.10+ (the `env/` folder contains a virtualenv).
- Node.js and npm/yarn for frontend tooling.
- Redis server(s) for queues and caching.
- MariaDB/PostgreSQL for Frappe sites (configured per-site under `sites/`).

Getting started (development)

1. Open a terminal in the repository root.
2. Activate the provided virtualenv (if you want to reuse it):

```bash
source env/bin/activate
```

3. Install Python dependencies for the apps you are working on. For example (project-root):

```bash
pip install -r frappe/requirements.txt
pip install -r erpnext/requirements.txt
```

4. Install Node dependencies for frontend assets where applicable, e.g. `frappe` or `builder`:

```bash
cd frappe
npm install
cd ../builder
npm install
```

5. Ensure Redis and a database server are running. Check `config/` for example Redis configs.

6. Create a site and install apps (example using bench commands if `bench` is available):

```bash
# from bench root (if bench CLI installed)
bench new-site mysite.local
bench --site mysite.local install-app frappe
bench --site mysite.local install-app erpnext
```

Running services (development)

You can start the usual bench services (webserver, worker, scheduler) using the bench CLI or the included `Procfile`:

```bash
# with bench
bench start

# or use foreman/honcho with Procfile
pip install honcho
honcho start -f Procfile
```

Project structure notes

- `apps/` — top-level apps directory. Each app contains its own README, packaging metadata, and tooling.
- `config/` — redis and other configuration examples; useful to mirror for production deployment.
- `env/` — virtualenv; commit of virtualenv is present in this copy. For reproducible builds, prefer using a requirements file or `pyproject.toml`.
- `sites/` — site configuration, assets, and `apps.txt` listing installed apps.

Testing

- Many apps include their own tests. Run Python test runners from the app directory. Example:

```bash
cd frappe
pytest
```

Code style & linting

- JavaScript: use project `package.json` scripts (e.g., `npm run lint`).
- Python: run `flake8`/`ruff` or the linters configured in `pyproject.toml`.

Contributing

1. Fork the repository and create a feature branch.
2. Run unit and integration tests locally.
3. Follow existing code style and tests.
4. Open a pull request with a clear description and relevant test coverage.

License

Check the individual app folders for license files. The repository includes `LICENSE` files inside apps such as `frappe/` and `builder/` — respect those licenses for the relevant components.

Useful files

- `Procfile` — process definitions for development run via foreman/honcho.
- `apps/apps.txt` and `sites/apps.txt` — list of installed apps for sites.
- `pyproject.toml` and `package.json` files inside app folders — dependency and build info.

Next steps (recommended)

- Add a top-level `requirements.txt` or `pyproject.toml` that lists pinned Python deps for reproducible installs.
- Provide a `CONTRIBUTING.md` with the repo's contribution workflow.
- Add CI (GitHub Actions) to run tests and linters on PRs.
- Link to specific app READMEs (e.g., `frappe/README.md`, `erpnext/README.md`) for app-level docs.

Contact

For questions about individual apps, see their README files in `apps/<appname>/` or open an issue in this repo.
