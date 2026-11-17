# docker-starter 🐳

Spin up **PostgreSQL** and **SQL Server** locally with a single command — each one already seeded with sample tables and data. Handy for prototyping, practicing SQL, or giving an app a database to talk to without installing anything.

## What's included

| Service | Image | Host port | Seed script |
|---|---|---|---|
| PostgreSQL | `postgres` (latest) | `5431` | `db_data/init.sql` — `Villains`, `Superpowers` |
| SQL Server 2022 | `mcr.microsoft.com/mssql/server:2022-CU12` | `1433` | `sql-server-scripts/init.sql` — `Heroes`, `Sidekicks`, `Weapons` |

- **Postgres** runs every `.sql` file in `db_data/` on first start (via `/docker-entrypoint-initdb.d`).
- **SQL Server** waits until the engine accepts connections, then runs `init.sql` with `sqlcmd`.

## Usage

```bash
docker compose up -d      # start both databases
docker compose logs -f    # watch the SQL Server init script run
docker compose down       # stop
```

Start just one of them:

```bash
docker compose up -d db          # PostgreSQL only
docker compose up -d sqlserver   # SQL Server only
```

## Connecting

| | PostgreSQL | SQL Server |
|---|---|---|
| Host | `localhost` | `localhost` |
| Port | `5431` | `1433` |
| User | `postgresUser` | `sa` |
| Password | `example123asd` | `YourStrongPassword123` |
| Database | `test` | `master` |

```bash
psql -h localhost -p 5431 -U postgresUser -d test
```

> ⚠️ These credentials are for **local development only**. Change them before using this setup anywhere else.

## Customizing

- Add your own schema/data by editing or adding `.sql` files in `db_data/` (Postgres) or `sql-server-scripts/init.sql` (SQL Server).
- Postgres init scripts only run on an **empty** data directory — recreate the container to re-run them: `docker compose down && docker compose up -d`.
