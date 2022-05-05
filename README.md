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
  - Host ($HEROKU_HOST)
  - Database ($HEROKU_DATABASE)
  - User ($HEROKU_USER)
  - Password ($HEROKU_PASSWORD)

## Get your Supabase Host
- Log into the Supabase Dashboard (https://supabase.com)
- Select your project
- Select Settings / Database
- Under Connection Info / Host, note your Host ($SUPABASE_HOST)

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

## (Optional) Post Processing
If you're using graphql with your Supabase projects, you may need to issue the following SQL command in your Supabase Dashboard (Query Editor) to make sure the new tables show up:

```sql
select graphql.rebuild_schema();
```

