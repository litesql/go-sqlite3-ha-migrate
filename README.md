# go-sqlite3-ha-migrate

[![Go Reference](https://pkg.go.dev/badge/github.com/litesql/go-sqlite3-ha-migrate/v4.svg)](https://pkg.go.dev/github.com/litesql/go-sqlite3-ha-migrate/v4)

[go-sqlite3-ha](https://github.com/litesql/go-sqlite3-ha) database driver for [golang-migrate](https://github.com/golang-migrate/migrate/).

This is a drop-in replacement for the upstream [`sqlite3`](https://github.com/golang-migrate/migrate/tree/master/database/sqlite3) driver, using [litesql/go-sqlite3](https://github.com/litesql/go-sqlite3) instead of `mattn/go-sqlite3`.

## Usage

```
sqlite3://path/to/database?query
```

Unlike other migrate database drivers, the sqlite3 driver will automatically wrap each migration in an implicit transaction by default. Migrations must not contain explicit `BEGIN` or `COMMIT` statements.

### URL query parameters

| URL Query | `Config` field | Description |
|---|---|---|
| `x-migrations-table` | `MigrationsTable` | Name of the migrations table. Defaults to `schema_migrations`. |
| `x-no-tx-wrap` | `NoTxWrap` | Disable implicit transactions when `true`. Migrations may, and should, contain explicit `BEGIN` and `COMMIT` statements. |

### With URL

```go
import (
    "github.com/golang-migrate/migrate/v4"
    _ "github.com/litesql/go-sqlite3-ha-migrate/v4"
)

m, err := migrate.New("file:///path/to/migrations", "sqlite3:///path/to/database")
```

### With existing database instance

```go
import (
    "database/sql"

    _ "github.com/litesql/go-sqlite3"
    sqlite3migrate "github.com/litesql/go-sqlite3-ha-migrate/v4"
)

db, err := sql.Open("sqlite3", "/path/to/database")

driver, err := sqlite3migrate.WithInstance(db, &sqlite3migrate.Config{})

m, err := migrate.NewWithDatabaseInstance("file:///path/to/migrations", "main", driver)
```

## Installation

```sh
go get github.com/litesql/go-sqlite3-ha-migrate/v4
```

## License

MIT
