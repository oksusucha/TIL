### ORM query selected 후 response를 특수한 struct 로 변경하기 
```go

type GormSelectedResult struct {
  ID        string `gorm:"type:varchar(100);unique_index;not null"`
	Name      string `gorm:"type:varchar(20);not null"`
	Email     string `gorm:"type:varchar(100);not_null"`
}

type CustomResponse struct {
  ID string `json:id`
}

var gormSelectedResult GormSelectedResult
... 중략 ...

var res []CustomResponse

bGormSelectedResult, _ := json.Marshal(&gormSelectedResult)

json.Unmarshal(bGormSelectedResult, &res)

```

> 위와 같이 하게되면 특정한 response 로 만들 수 있다
