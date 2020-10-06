*** makefile에서 디렉토리의 존재 여부 확인

```makefile
TARGET_DIR = ${GOPATH}/src/github.com/grpc-ecosystem/grpc-gateway

download:
	if [ "$(wildcard $(TARGET_DIR))" == "" ]; then \
		git clone https://github.com/grpc-ecosystem/grpc-gateway.git $(TARGET_DIR); \
	fi
```
