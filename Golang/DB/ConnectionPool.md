### Connection Pool

```go
// Connection 을 생성한다.
db, err = gorm.Open("mysql", dbUrl)
if err != nil {
    fmt.Println(err)
}
```
```go
// Connection Pool 정의
db.DB().SetMaxIdleConns(5)
db.DB().SetMaxOpenConns(5)
db.DB().SetConnMaxLifetime(time.Hour)
```
```go
// Get Transaction
trx := db.Begin()

// Connection release
db.Commit()
db.Rollback()

// 또는
trx.Commit()
trx.Rollback()
```
