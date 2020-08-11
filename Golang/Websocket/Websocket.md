### Redis Pub/Sub 데이터를 Websocket 으로 전달
```go
// websocket.go

// 클라이언트에게 메세지 전달 시 오류가 발생 하게 되었을때 서버에서 클라이언트 제거 목적으로 map을 활용
var Clients = make(map[*websocket.Conn]bool)

// 채널
var Broadcast = make(chan []byte)

// http upgrade
var Upgrader = websocket.Upgrader{
	CheckOrigin: func(r *http.Request) bool {
		return true
	},
	EnableCompression: true,
}

func MsgSendHandler() {
	for {
        	// 채널에서 메세지 전달 받음 
		msg := <-Broadcast
        
        	// 연결되어 있는 모든 클라이언트에게 메세지 전달
		for target := range Clients {
			if err := target.WriteJSON(msg); err != nil {
				target.Close()
				delete(Clients, target)
			}
		}
	}
}
```
```go
// main.go

func main() {
    // 고루틴으로 채널 구독 
    go SetupSubs()
}

func SetupSubs() {
    	// 어디서 구독된 정보를 가져올지에 대한 레디스 채널 정보
	_, ch := redisHandler.SystemSubscribe("sub-channel-*")

	go func() {
		for {
            		// 레디스로 넘어온 데이터를 메세지에 담음
			msg := <-ch
            		// 메세지를 다시 websocket의 채널로 넘겨줌
			websocket.Broadcast <- msg
		}
	}()
}
```
```go
// redisHandler.go

type Event struct {
	Channel string
	Client  *redis.Client
	Pubsub  *redis.PubSub
}

func SystemSubscribe(channel string) (*Event, chan []byte) {
    	// redis connection
	client := GetRedisConnection(2) // redis DB 번호
    
	pubsub := client.PSubscribe(channel)
	e := &Event{
		Channel: channel,
		Client:  client,
		Pubsub:  pubsub,
	}

	c := make(chan []byte)
	go func() {
		for {
			msg, err := e.Pubsub.Receive()
			if err != nil {
				log.Error().Err(err)
				return
			}
			switch msg := msg.(type) {
			case *redis.Message:
               			// 필요한 데이터만 채널로 넘겨줌
				c <- []byte(msg.Payload)
			default:
			}
		}
	}()

	return e, c
}
```

```go
// webserver 구동 시
// 여기서 r 은 gin 을 활용한 router가 됨.

r.GET("/ws", func() {
    ws, err := websocket.Upgrader.Upgrade(c.Writer, c.Request, nil)
    // 접속한 유저는 웹소켓의 클라이언트 목록에 정상 등록
    websocket.Clients[ws] = true

    if err != nil {
        log.Error().Err(err)
    }
})
```

> 어디에 쓸까?
+ 대쉬보드의 정보성 데이터 (서버 상태정보, 동시접속유저 등)
+ 채팅
+ 주식 등 실시간 변동성 데이터

> 고민할 점?
+ SetupSubs() 에서 채널에서 나온 데이터를 받아서 보내는 과정을 전달 과정으로 한번에 넘길 수 없을까?
