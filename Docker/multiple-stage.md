### 다중 FROM

```docker
FROM continuumio/miniconda3:latest AS miniconda
RUN conda create --name multiple_stage python=3.6

FROM busybox
COPY --from=miniconda /opt/conda /opt/conda
ENV PATH /opt/conda/bin:$PATH
```

+ 위와 같은 방법으로 multiple-stage 를 활용하여 Dockerfile을 작성 할 수 있다.
+ 빌드할때와 구동할때 의존성을 분리할 수 있다.
+ Docker image의 용량이 작아진다.
