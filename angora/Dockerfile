FROM unifuzz/unibench:angora_step2

RUN cd /unibench/binutils-2.28 && ./configure --disable-shared &&\
    make -j &&\
    cp binutils/objdump /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 ASAN_OPTIONS="detect_leaks=0" make -j &&\
    cp binutils/objdump /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/libpcap-1.8.1 && ./configure --disable-shared &&\
    make -j &&\
    cd /unibench/tcpdump-4.8.1 && ./configure &&\
    make -j &&\
    cp tcpdump /d/p/angora/fast/ &&\
    cd /unibench/libpcap-1.8.1 &&\
    make clean && USE_TRACK=1 make -j &&\
    cd /unibench/tcpdump-4.8.1 &&\
    make clean && USE_TRACK=1 make -j &&\
    cp tcpdump /d/p/angora/taint/ &&\
    make clean && cd /unibench/libpcap-1.8.1 && make clean

# this will add /d/p/angora/fast/ffmpeg and /d/p/angora/taint/ffmpeg
RUN wget --quiet https://gitlab.com/unifuzz/unibench_build/raw/master/ffmpeg/angora.tar.gz &&\
    tar xf angora.tar.gz -C / &&\
    rm angora.tar.gz

RUN cd /unibench/jasper-2.0.12 && cmake -DJAS_ENABLE_SHARED=OFF -DALLOW_IN_SOURCE_BUILD=ON . &&\
    make -j &&\
    cp src/appl/imginfo /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp src/appl/imginfo /d/p/angora/taint/ &&\
    make clean

RUN ls -alh /d/p/angora/*