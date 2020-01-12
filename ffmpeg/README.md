## Building ffmpeg

### Why this isolated part?

Both Dockerhub and travis-ci cannot finish the build for ffmpeg (exceeding memory limit and timeout), I have to build this part manually.

This folder is not part of travis build.

### Binaries built for usage

I will put binary built on another branch of this repo.

TODO: add link and fix other Dockerfile

### If you want build it

You can repeat this build on your own machine (Memory more than 8GB is recommended, have patience).

```
TAG=afl
docker build -f Dockerfile.$TAG -t unibench_tmp:$TAG .
docker run -it --rm -v `pwd`:/data unibench_tmp:$TAG tar cf /data/${TAG}.tar.gz /d/p
docker rmi unibench_tmp:$TAG
```

