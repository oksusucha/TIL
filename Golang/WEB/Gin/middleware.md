### REST로 들어가기 전 미들웨어로 동작
```go
package rabbitmq

import (
	"github.com/gin-gonic/gin"
	"github.com/streadway/amqp"
	"os"
)

// RabbitMQ connectino 및 channel 초기화
func RabbitConnection() gin.HandlerFunc {
	return func(c *gin.Context) {
		conn, ch, err := getConnection()

		if err != nil {
			c.Abort()
		} else {
			// *gin.Context 에 넣어주는 부분
			c.Set("amqp_conn", conn)
			c.Set("amqp_ch", ch)

			c.Next()
		}
	}
}

func getConnection() (*amqp.Connection, *amqp.Channel, error) {
	destRabbitMQ := os.Getenv("RABBIT_MQ")

	connection, err := amqp.Dial(destRabbitMQ)

	if err != nil {
		return nil, nil, err
	}

	channel, err := connection.Channel()

	if err != nil {
		return nil, nil, err
	}

	return connection, channel, err
}
```
