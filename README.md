# heroku-to-supabase
Heroku to Supabase Migration Guide

## Prerequisites
- A Heroku project with a Postgres database you want to migrate to Supabase
- A Supabase project you want to migrate into
- pg_dump command-line tool installed
- psql command-line tool installed

## Get your Heroku Database Credentials
- Log into Heroku (https://heroku.com)
- Select your project
- Click `Resources` in the menu
- Select your `Heroku Postgres` database
- Click `Settings` in the menu
- Click `View Credentials`
- Save your credentials: 
  - Host (`$HEROKU_HOST`)
  - Database (`$HEROKU_DATABASE`)
  - User (`$HEROKU_USER`)
  - Password (`$HEROKU_PASSWORD`)

## Get your Supabase Host
- Log into the Supabase Dashboard (https://supabase.com)
- Select your project
- Select Settings / Database
- Under Connection Info / Host, note your Host (`$SUPABASE_HOST`)

## Dump your Heroku Database to a file

```
pg_dump --clean --if-exists --quote-all-identifiers \
 -h $HEROKU_HOST -U $HEROKU_USER -d $HEROKU_DATABASE \
 --no-owner --no-privileges > heroku_dump.sql
```

## Import the data to your Supabase project

```
psql -h $SUPABASE_HOST -U postgres -f heroku_dump.sql 
```

## Additional Options

- To only migrate a single database schema, add the `--schema=PATTERN` parameter to your `pg_dump` command.
- To exclude a schema: `--exclude-schema=PATTERN`.
- To only migrate a single table:  `--table=PATTERN`.
- To exclude a tables: `--exclude-table=PATTERN`.

Run `pg_dump --help` for a full list of options.