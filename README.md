# Flyway OSS Demo

A simple demo of using Flyway CLI (OSS) with Docker to manage Postgres Schema Migration.
It also includes a Dockerfile that you can use to connect to RDS with SSL/TLS.

To use the Dockerfile for managing postgres Schema Migration of an RDS,

1. Build the image and push it to ECR.
2. Create a Fargate task definition from it.
3. Run it as a one-off task (aws ecs run-task) with a command override like ["-environment=rds", "migrate"]. The image's entrypoint is already flyway, so the override is only the arguments.

For more details, please refer to my blog [Manage Postgres Schema Migration with Flyway OSS](https://medium.com/@itsuki.enjoy/manage-postgres-schema-migration-with-flyway-oss-1d3c59a10359)

## Install Flyway

```bash
brew install flyway
```


## Set up PostgresSQL DB Locally

```bash
brew install postgresql@18
# To start postgresql@18 now and restart at login:
brew services start postgresql@18
# if you don't want/need a background service you can just run:
# LC_ALL="en_US.UTF-8" /opt/homebrew/opt/postgresql@18/bin/postgres -D /opt/homebrew/var/postgresql@18

# Create a postgres role with full access
psql -h localhost -p 5432  postgres
postgres-# create user postgres with password 'postgres';
postgres-# grant pg_read_all_data, pg_write_all_data to postgres;
postgres-# grant usage, create on schema public to postgres;

# stop postgres
brew services stop postgresql@18
```

## Flyway basic commands

By default, the configuration will be read from `conf/flyway.toml` file, relative to the installation, not the current folder.

```bash
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' info

# test connection
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' testConnection

# baseline against an existing DB
# https://documentation.red-gate.com/flyway/flyway-concepts/baselines
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' baseline

# Create a migration (if using redGate version, `flyway add`)
# Naming pattern: https://www.red-gate.com/blog/flyway-naming-patterns-matter/
touch "flyway/migrations/V$(date +%s)__<description>.sql"

# run all pending migrations
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' migrate

# undo all migrations
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' clean

# view migrations
flyway -workingDirectory='flyway' -configFiles='conf/flyway.toml' info

```

## Flyway Docker

Docker OSS: https://documentation.red-gate.com/flyway/reference/usage/flyway-open-source

```bash
# build image
docker build -t flyway:latest flyway
# run command.
# The command override is only the arguments as the image's entrypoint is already `flyway`.
docker run --rm flyway:latest info
docker run --rm flyway:latest testConnection
docker run --rm flyway:latest migrate
```
