### Add ARG
```Shell
# In Dockerfile
ARG CACHE_BUST
```

### Build dockerfile
```Shell
# Have to replace IMAGE_NAME:TAG
docker build -t IMAGE_NAME:TAG --build-arg CACHE_BUST=$(date +%s) .
```
