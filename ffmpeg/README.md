## Building ffmpeg

### Why this isolated part?

Both Dockerhub and travis-ci cannot finish the build for ffmpeg (exceeding memory limit and timeout), I have to build this part manually.

This folder is not part of travis build.

### Binaries built for usage

I have put built binary packages here using Git LFS.

As the traffic is limited for GitHub (only 1 GB/month), I also mirrored this repository [here](https://gitlab.com/unifuzz/unibench_build/tree/master/ffmpeg).

So, in the [Dockerfile of afl](https://github.com/UNIFUZZ/unibench_build/blob/master/afl/Dockerfile), I have added these lines:

```
# this will add /d/p/justafl/ffmpeg and /d/p/aflasan/ffmpeg
RUN wget https://gitlab.com/unifuzz/unibench_build/raw/master/ffmpeg/afl.tar.gz &&\
    tar xf afl.tar.gz -C / &&\
    rm afl.tar.gz
```

### If you want build it

You can repeat this build on your own machine (Memory more than 8GB is recommended, have patience).

```
TAG=afl
docker build -f Dockerfile.$TAG -t unibench_tmp:$TAG .
docker run -it --rm -v `pwd`:/data unibench_tmp:$TAG tar cf /data/${TAG}.tar.gz /d/p
docker rmi unibench_tmp:$TAG
```

Specially, coverage build need the build folder, so `coverage.tar.gz` is created by:

```
docker run -it --rm -v `pwd`:/data unibench_tmp:coverage tar cf /data/coverage.tar.gz /unibench/ffmpeg-4.0.1
```
