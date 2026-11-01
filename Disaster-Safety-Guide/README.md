# Disaster Safety Guide

An offline-first Progressive Web App for disaster preparedness, first-aid learning, emergency contacts, household planning, and emergency-kit tracking.

The project also includes a protected administration dashboard, MySQL content-management schema, feedback review workflow, audit logging, content version checks, API documentation, and automated backend checks.

Visitors can optionally create an account, sign in, and maintain a basic profile and language preference. Public emergency guidance never requires an account; household plans and checklists remain device-local for offline access and privacy.

## Project overview

Disaster Safety Guide gives individuals and families practical information and tools to prepare for emergencies. It brings disaster guidance, first-aid learning, emergency contacts, and household preparedness features together in a mobile-friendly web app.

### Main features

- Step-by-step disaster guides with preparation, response, and recovery information.
- First-aid learning guides and regional emergency contacts.
- An emergency-kit checklist and household safety plan.
- Search, saved guides, an awareness quiz, and English, Telugu, and Hindi language options.
- Offline access to the application and bundled guidance after the first successful load.
- Optional user accounts for a basic profile and preferred language.
- An administrator dashboard for emergency-contact management and feedback review.
- A feedback form for reporting information, contact, translation, and accessibility issues.

### Deployment status

The frontend, backend, and database have been deployed. The frontend is a static PWA, the backend is a Node.js/Express API, and MySQL stores server-managed records. Deployment URLs and environment-specific health are not included here; configure hosting secrets outside source control and verify each service in its hosting environment.

The app does not send live alerts or contact emergency services on a user's behalf. Offline household plans, checklist progress, bookmarks, and language preference are stored in the user's browser rather than uploaded.

## Why this stack

- **Vanilla HTML, CSS and JavaScript PWA:** very small download, fast on lower-powered phones, and easy to cache for offline use.
- **Node.js + Express:** a minimal server with little framework overhead.
- **MySQL:** maintainable emergency-contact and content-review data, compatible with MySQL Workbench.
- **Local browser storage:** private plans, bookmarks and checklists remain useful without Wi-Fi or a database connection.

The emergency experience never depends on the server or MySQL after the first successful load.

## Run locally

1. Install Node.js 20 or newer.
2. Run `npm install`.
3. Copy `.env.example` to `.env`.
4. Optional: open `database/schema.sql` in MySQL Workbench and run it, then update `.env` with your local MySQL password.
5. Run `npm start`.
6. Open `http://localhost:3000`.

Without MySQL configuration, the site still runs using its bundled offline content.

## Administrator setup

In the local `.env` file, replace the example values for:

```text
ADMIN_EMAIL=your-admin-email
ADMIN_PASSWORD=use-a-strong-local-password
SESSION_SECRET=use-a-long-random-secret
```

Restart the server and open `http://localhost:3000/#admin`. Never commit `.env` or use the example password for a public deployment.

The administration dashboard manages verified emergency contacts and feedback, displays content-review counts, and documents the reviewed publishing workflow. The MySQL schema also contains normalized disaster, translation, first-aid, kit, quiz, content-version and audit tables for continued development.

## Vercel and hosted MySQL

The server uses the separate `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, and `DB_NAME` variables. For providers that require TLS, set `DB_SSL_CA` to the full CA certificate text; the server verifies the database certificate.

To apply the complete schema locally, run `npm run migrate` after setting those variables in `.env`. The migration script uses the configured database and does not attempt to create or switch databases.

The GitHub Actions workflow at `.github/workflows/database-migrate.yml` applies the schema on pushes to `main`. Add `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`, and `DB_SSL_CA` as GitHub Actions repository secrets. Change the workflow branch filter if your production branch is not named `main`. The schema uses `CREATE TABLE IF NOT EXISTS` and upserts its seed rows, so this workflow can safely be rerun for the current schema. Review schema changes before deploying them; do not put database credentials in workflow files.

Vercel and GitHub Actions have separate environment settings: configure the same database variables in both places. Setting Vercel variables alone does not give the GitHub workflow access to them.

## Quality checks

Run:

```powershell
npm run check
npm test
```

Architecture and API documentation are in `docs/architecture.md` and `docs/api.md`.

## Offline test

1. Open the site once while online/local server is running.
2. In browser developer tools, confirm the service worker is active.
3. Select **Offline** in the Network panel, then refresh.
4. Test disaster guides, first aid, contacts, checklist, safety plan, language preference, search and bookmarks.

Service workers require HTTPS in production; localhost is allowed for development.

## Safety review before public release

The software includes visible review notices because public deployment requires:

- verification of every emergency number for the intended region;
- qualified medical review of first-aid instructions;
- authoritative source and review metadata for every guide;
- reviewed Telugu and Hindi safety-content translations;
- device and accessibility testing, including offline cold starts.

The interface does not claim to contact responders or provide live alerts.
