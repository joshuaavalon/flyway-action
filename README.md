# Flyway Action

This action runs [Flyway v8][flyway] to migrate database. It aims to migrate database for testing in workflows.

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
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v2
      - uses: joshuaavalon/flyway-action@v1
        with:
          url: jdbc:postgresql://postgres:5432/postgres
          user: postgres
          password: postgres
      - run: echo 'testing'
```

Currently, it supports `url`, `user`, `password`, and `locations`. `locations` are default to `filesystem:./sql`.

For details, please check out Flyway [documentation].

[flyway]: https://flywaydb.org/
[documentation]: https://flywaydb.org/documentation/configuration/parameters/
