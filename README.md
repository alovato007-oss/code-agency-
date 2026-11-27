# Code Agency

Repository for the multi-agent resale system's initial database schema.

## Repository contents

- `database/schema.sql` — core PostgreSQL tables and indexes for products, agent runs, pricing history, inventory events, customer messages, and revenue snapshots.
- `.github/ISSUE_TEMPLATE/recommend-a-resource.yml` — GitHub issue form for resource recommendations.

## Database schema

The SQL file defines the initial schema only. Review it against the target database and add a versioned migration before applying it to a live environment.

## Safety

Do not apply this schema directly to production without a reviewed migration, backup, and staging validation.
