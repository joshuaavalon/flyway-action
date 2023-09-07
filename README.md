# Flyway Action

This action runs [Flyway v9][flyway] to migrate database. It aims to migrate database for testing in workflows.

## Usage

```yaml
name: Main
on:
  - push
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres
        env:
          POSTGRES_DB: db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v2
      - uses: joshuaavalon/flyway-action@v3.0.0
        with:
          url: jdbc:postgresql://postgres:5432/db
          user: user
          password: password
      - run: echo 'testing'
```

Currently, it supports `url`, `user`, `password`, `initSql` and `locations`. `locations` are default to `filesystem:./sql`.

Extra configurations can be passed via environment variables.

```yaml
- uses: joshuaavalon/flyway-action@v3.0.0
  with:
    url: jdbc:postgresql://postgres:5432/db
    user: user
    password: password
  env:
    FLYWAY_VALIDATE_MIGRATION_NAMING: true
```

For details, please check out Flyway [documentation].

[flyway]: https://flywaydb.org/
[documentation]: https://flywaydb.org/documentation/configuration/parameters/

## Version matrix

| Action version | Flyway version running      |
|----------------|-----------------------------|
| @v3.0.0        | flyway community edition v9 |
| @v2.1.0        | flyway community edition v8 |
| @v2            | flyway community edition v7 |
| @v1            | flyway community edition v7 |

## Note

### Database compatibility

> Flyway Community Edition `8.0.0-beta1` dropped support for databases older than 5 years, including MySQL `5.7`.  
> The minimum supported version of MySQL was increased from `5.7` to `8.0`.

For details, please check out Flyway [release note](https://documentation.red-gate.com/fd/release-notes-for-flyway-engine-179732572.html#8.0.0-beta1).
