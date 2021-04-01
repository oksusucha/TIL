### Using upx

```Shell
# Upx download
ADD https://github.com/upx/upx/releases/download/v3.96/upx-3.96-amd64_linux.tar.xz /usr/local
RUN xz -d -c /usr/local/upx-3.96-amd64_linux.tar.xz | tar -xOf - upx-3.96-amd64_linux/upx > /bin/upx \
    && chmod a+x /bin/upx

# Copy source files
COPY . .

# Build 
RUN go mod tidy \
    && go get -u -d -v ./... \
    && CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags '-s -w' -o main cmd/server.go \
    && strip --strip-unneeded main \
    && /bin/upx --lzma main
```
