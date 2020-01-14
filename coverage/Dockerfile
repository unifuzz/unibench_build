FROM zjuchenyuan/base
RUN git clone https://github.com/UNIFUZZ/unibench /unibench &&\
    cd /unibench && \
    mkdir mp3gain-1.5.2 && cd mp3gain-1.5.2 && mv ../mp3gain-1.5.2.zip ./ && unzip -q mp3gain-1.5.2.zip && rm mp3gain-1.5.2.zip && cd .. &&\
    ls *.zip|xargs -i unzip -q '{}' &&\
    ls *.tar.gz|xargs -i tar xf '{}' &&\
    rm -r .git/ *.tar.gz *.zip &&\
    mv SQLite-8a8ffc86 SQLite-3.8.9 && mv binutils_5279478 binutils-5279478 && mv libtiff-Release-v3-9-7 libtiff-3.9.7 &&\
    ls -alh

ENV CC="gcc -fprofile-arcs -ftest-coverage" CXX="g++ -fprofile-arcs -ftest-coverage"

RUN mkdir -p /d/p/cov

RUN cd /unibench/exiv2-0.26 && cmake -DEXIV2_ENABLE_SHARED=OFF . &&\
    make -j &&\ 
    cp bin/exiv2 /d/p/cov/

RUN apt update && apt install -y libglib2.0-dev gtk-doc-tools libtiff-dev libpng-dev &&\
    cd /unibench/gdk-pixbuf-2.31.1 && ./autogen.sh --enable-static=yes --enable-shared=no --with-included-loaders=yes &&\
    make -j &&\
    cp gdk-pixbuf/gdk-pixbuf-pixdata /d/p/cov/

RUN cd /unibench/jasper-2.0.12 && cmake -DJAS_ENABLE_SHARED=OFF -DALLOW_IN_SOURCE_BUILD=ON . &&\
    make -j &&\
    cp src/appl/imginfo /d/p/cov/

RUN cd /unibench/jhead-3.00 &&\
    make -j &&\
    cp jhead /d/p/cov/

RUN cd /unibench/libtiff-3.9.7 && ./autogen.sh && ./configure --disable-shared &&\
    make -j &&\
    cp tools/tiffsplit /d/p/cov/

RUN cd /unibench/lame-3.99.5 && ./configure --disable-shared &&\
    make -j &&\
    cp frontend/lame /d/p/cov/

RUN cd /unibench/mp3gain-1.5.2 && sed -i 's/CC=/CC?=/' Makefile &&\
    make -j &&\
    cp mp3gain /d/p/cov/

RUN cd /unibench/swftools-0.9.2/ && ./configure &&\
    make -j &&\
    cp src/wav2swf /d/p/cov/

RUN cd /unibench/flvmeta-1.2.1 && cmake . &&\
    make -j &&\
    cp src/flvmeta /d/p/cov/

RUN cd /unibench/Bento4-1.5.1-628 && cmake . &&\
    make -j &&\
    cp mp42aac /d/p/cov/

RUN cd /unibench/cflow-1.6 && ./configure &&\
    make -j &&\
    cp src/cflow /d/p/cov/

RUN cd /unibench/ncurses-6.1 && ./configure --disable-shared &&\
    make -j &&\
    cp progs/tic /d/p/cov/infotocap

RUN cd /unibench/jq-1.5 && ./configure --disable-shared &&\
    make -j &&\
    cp jq /d/p/cov/

RUN cd /unibench/mujs-1.0.2 &&\
    build=debug make -j &&\
    cp build/debug/mujs /d/p/cov/

RUN cd /unibench/xpdf-4.00 && cmake . &&\
    make -j &&\
    cp xpdf/pdftotext /d/p/cov/

RUN apt install -y tcl-dev &&\
    cd /unibench/SQLite-3.8.9 && ./configure --disable-shared --disable-amalgamation  &&\ 
    make -j &&\
    cp sqlite3 /d/p/cov/

RUN cd /unibench/binutils-5279478 &&\
    ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; ./configure --disable-shared && make -j; cd ..; done  &&\
    cd binutils  &&\
    ./configure --disable-shared &&\
    make nm-new &&\
    cp nm-new /d/p/cov/nm

RUN cd /unibench/binutils-2.28 && ./configure --disable-shared &&\
    make -j &&\
    cp binutils/objdump /d/p/cov/

RUN cd /unibench/libpcap-1.8.1 && ./configure --disable-shared &&\
    make -j &&\
    cd /unibench/tcpdump-4.8.1 && ./configure &&\
    make -j &&\
    cp tcpdump /d/p/cov/

RUN rm -r /unibench/ffmpeg-4.0.1 &&\
    wget --quiet https://gitlab.com/unifuzz/unibench_build/raw/master/ffmpeg/coverage.tar.gz &&\
    tar xf coverage.tar.gz -C / &&\
    rm coverage.tar.gz &&\
    cp /unibench/ffmpeg-4.0.1/ffmpeg_g /d/p/cov/ffmpeg

RUN ls -alh /d/p/*