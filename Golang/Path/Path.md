### Runtime, 호출된 위치의 절대 경로 가져오기
```go
func GetCurrentPath() string {
	_, b, _, _ := runtime.Caller(0)
	return path.Dir(b)
}
```
어디에 쓸까?
+ 설정파일을 읽어와야 하는 경우
  + 테스트 시작점에 따라 참조되는 경로가 다를때

