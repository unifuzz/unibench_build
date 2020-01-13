FROM zjuchenyuan/angora
RUN git clone https://github.com/UNIFUZZ/unibench /unibench &&\
    cd /unibench && \
    mkdir mp3gain-1.5.2 && cd mp3gain-1.5.2 && mv ../mp3gain-1.5.2.zip ./ && unzip -q mp3gain-1.5.2.zip && rm mp3gain-1.5.2.zip && cd .. &&\
    ls *.zip|xargs -i unzip -q '{}' &&\
    ls *.tar.gz|xargs -i tar xf '{}' &&\
    rm -r .git/ *.tar.gz *.zip &&\
    mv SQLite-8a8ffc86 SQLite-3.8.9 && mv binutils_5279478 binutils-5279478 && mv libtiff-Release-v3-9-7 libtiff-3.9.7 &&\
    ls -alh

RUN mkdir -p /d/p/angora/taint /d/p/angora/fast &&\
    apt update && apt install -y nasm libglib2.0-dev gtk-doc-tools libtiff-dev libpng-dev tcl-dev

COPY --from=unifuzz/unibench:gcc  /d/p/normal /d/p/normal

RUN ldd /d/p/normal/*|grep .so|awk '{print $3}'|grep .so|sort|uniq|sed 's#^/lib#/usr/lib#g'|sed 's#\.so.*$#.so#g'|grep -v libgcc_s.so|grep -v libstdc++.so|grep -v libc.so|grep -v libm.so|grep -v libpthread.so|xargs -i /angora/tools/gen_library_abilist.sh '{}' discard >> /tmp/abilist.txt

ENV ANGORA_TAINT_RULE_LIST=/tmp/abilist.txt

RUN cd /unibench/exiv2-0.26 && cmake -DEXIV2_ENABLE_SHARED=OFF . && make -j && cp bin/exiv2 /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j && cp bin/exiv2 /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/gdk-pixbuf-2.31.1 &&\
    ./autogen.sh --enable-static=yes --enable-shared=no --with-included-loaders=yes && make -j &&\
    cp gdk-pixbuf/gdk-pixbuf-pixdata /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp gdk-pixbuf/gdk-pixbuf-pixdata /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/jhead-3.00 &&\
    make -j &&\
    cp jhead /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp jhead /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/libtiff-3.9.7 && ./autogen.sh && ./configure --disable-shared &&\
    make -j &&\
    cp tools/tiffsplit /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp tools/tiffsplit /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/lame-3.99.5 && ./configure --disable-shared &&\
    make -j &&\
    cp frontend/lame /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp frontend/lame /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/mp3gain-1.5.2 && sed -i 's/CC=/CC?=/' Makefile &&\
    make -j &&\
    cp mp3gain /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp mp3gain /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/swftools-0.9.2/ && ./configure &&\
    sed -i 's/int inline ActionTagSize/int ActionTagSize/' ./lib/modules/swfaction.c &&\
    sed -i 's/byte inline PaethPredictor/byte PaethPredictor/' ./src/png2swf.c &&\
    make -j &&\
    cp src/wav2swf /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp src/wav2swf /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/flvmeta-1.2.1 && cmake . &&\
    make -j &&\
    cp src/flvmeta /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp src/flvmeta /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/Bento4-1.5.1-628 && cmake . &&\
    make -j &&\
    cp mp42aac /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp mp42aac /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/cflow-1.6 && ./configure &&\
    make -j &&\
    cp src/cflow /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp src/cflow /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/ncurses-6.1 && ./configure --disable-shared &&\
    make -j &&\
    cp progs/tic /d/p/angora/fast/infotocap &&\
    make clean && USE_TRACK=1 ASAN_OPTIONS="detect_leaks=0" make -j &&\
    cp progs/tic /d/p/angora/taint/infotocap &&\
    make clean

RUN ls -alh /d/p/*