
# Docker presentation:

### Code and examples from docker presentation:

[click here](https://github.com/n0npax/docker_demo)

## How securely store a temp secret in docker image:
 
Check this [docs](https://docs.docker.com/develop/develop-images/build_enhancements/#new-docker-build-secret-information)

## Multistage images

Why:
1. Because you can separate logic (data preparation, dependency install and final app) in nice way.
2. smaller final docker images (faster pull - faster recovery)

How can you use multiple stages in real live:
1. Install all deps in builder
2. build application
3. copy artefact to final stage

```dockerfile
FROM python:latest as builder
COPY . /app
RUN pip install /app/requirements.txt
FROM builder as app-builder
RUN pip wheel .
FROM python:alpine-latest
COPY --from=app-builder /app/my.whl /app/my.whl
...
```

why this is good?

Because you can keep small final image and keep a cache for build performance. In example you can build & tag only `app-builder` using `builder` cache.
```
docker build --cache-from=my-builder:latest --target app-builder -t app-builder:1.2.3
```
If cached `builder:latest` is ok, it will be used, if not docker will rebuild required layers.

Pain point: cache has to be present on node. In case `my-builder:latest` is not present, docker will rebuild without trying to pull.
Solution: `docker pull` before docker build `--cache-from`.