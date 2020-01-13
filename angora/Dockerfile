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

# TODO: add ffmpeg

RUN ls -alh /d/p/angora/*