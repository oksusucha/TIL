### 초단위 시간이 겹치는 현상이 발생 할 때 datetime 컬럼 변경

```go
// 기존
type HistoryLog struct {
  InsertTime     string `gorm:"type:datetime;not null"`
}

// 변경
type HistoryLog struct {
  InsertTime     string `gorm:"type:datetime(6);not null"`
}
```

```
// 입력 결과, 마이크로초 단위 입력 및 표현 가능
2020-08-13 09:20:06.589205
```

### 마이크로초 단위로 시간 출력
```go
// UTC 기준 현재시간을 포맷을 정하여 출력
func GetCurrentTime() string {
	return time.Now().UTC().Format("2006-01-02 15:04:05.999999")
}
```
