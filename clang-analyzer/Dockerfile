FROM zjuchenyuan/base
RUN git clone https://github.com/UNIFUZZ/unibench &&\
    cd unibench && \
    mkdir mp3gain-1.5.2 && cd mp3gain-1.5.2 && mv ../mp3gain-1.5.2.zip ./ && unzip -q mp3gain-1.5.2.zip && rm mp3gain-1.5.2.zip && cd .. &&\
    ls *.zip|xargs -i unzip -q '{}' &&\
    ls *.tar.gz|xargs -i tar xf '{}' &&\
    rm -r .git/ *.tar.gz *.zip &&\
    mv SQLite-8a8ffc86 SQLite-3.8.9 && mv binutils_5279478 binutils-5279478 && mv libtiff-Release-v3-9-7 libtiff-3.9.7 &&\
    ls -alh

RUN apt update && apt install -y clang-tools-6.0 &&\
    mkdir -p /report /d/p/clang6

RUN cd /unibench/exiv2-0.26 && scan-build-6.0 -o /report/exiv2 cmake -DEXIV2_ENABLE_SHARED=OFF . &&\
    scan-build-6.0 -o /report/exiv2 make -j4 &&\
    cp bin/exiv2 /d/p/clang6/ &&\
    make clean

RUN apt update && apt install -y libglib2.0-dev gtk-doc-tools libtiff-dev libpng-dev &&\
    cd /unibench/gdk-pixbuf-2.31.1 && scan-build-6.0 -o /report/gdk-pixbuf ./autogen.sh --enable-static=yes --enable-shared=no --with-included-loaders=yes &&\
    scan-build-6.0 -o /report/gdk-pixbuf make -j4 &&\
    cp gdk-pixbuf/gdk-pixbuf-pixdata /d/p/clang6/ &&\
    make clean

RUN cd /unibench/jasper-2.0.12 &&\
    scan-build-6.0 -o /report/jasper cmake -DJAS_ENABLE_SHARED=OFF -DALLOW_IN_SOURCE_BUILD=ON . &&\
    scan-build-6.0 -o /report/jasper make -j &&\
    cp src/appl/imginfo /d/p/clang6/ &&\
    make clean

RUN cd /unibench/jhead-3.00 &&\
    scan-build-6.0 -o /report/jhead make -j &&\
    cp jhead /d/p/clang6/ &&\
    make clean

RUN cd /unibench/libtiff-3.9.7 &&\
    scan-build-6.0 -o /report/libtiff ./autogen.sh && scan-build-6.0 -o /report/libtiff ./configure --disable-shared &&\
    scan-build-6.0 -o /report/libtiff make -j &&\
    cp tools/tiffsplit /d/p/clang6/ &&\
    make clean

RUN cd /unibench/lame-3.99.5 &&\
    scan-build-6.0 -o /report/lame ./configure --disable-shared &&\
    scan-build-6.0 -o /report/lame make -j &&\
    cp frontend/lame /d/p/clang6/ &&\
    make clean

RUN cd /unibench/mp3gain-1.5.2 && sed -i 's/CC=/CC?=/' Makefile &&\
    scan-build-6.0 -o /report/mp3gain make -j &&\
    cp mp3gain /d/p/clang6/ &&\
    make clean

RUN cd /unibench/swftools-0.9.2/ &&\
    scan-build-6.0 -o /report/swftools ./configure &&\
    scan-build-6.0 -o /report/swftools make -j &&\
    cp src/wav2swf /d/p/clang6/ &&\
    make clean

RUN apt install -y nasm &&\
    cd /unibench/ffmpeg-4.0.1 &&\
    scan-build-6.0 -o /report/ffmpeg ./configure --disable-shared --cc="/usr/share/clang/scan-build-6.0/libexec/ccc-analyzer" --cxx="/usr/share/clang/scan-build-6.0/libexec/ccc-analyzer" &&\
    scan-build-6.0 -o /report/ffmpeg make -j &&\
    cp ffmpeg_g /d/p/clang6/ffmpeg &&\
    make clean

RUN cd /unibench/flvmeta-1.2.1 &&\
    scan-build-6.0 -o /report/flvmeta cmake . &&\
    scan-build-6.0 -o /report/flvmeta make -j &&\
    cp src/flvmeta /d/p/clang6/ &&\
    make clean

RUN cd /unibench/Bento4-1.5.1-628 &&\
    scan-build-6.0 -o /report/bento4 cmake . &&\
    scan-build-6.0 -o /report/bento4 make -j &&\
    cp mp42aac /d/p/clang6/ &&\
    make clean

RUN cd /unibench/cflow-1.6 &&\
    scan-build-6.0 -o /report/cflow ./configure &&\
    scan-build-6.0 -o /report/cflow make -j &&\
    cp src/cflow /d/p/clang6/ &&\
    make clean

# the last demo binary build will fail due to misusing C compiler for C++ code, but we can ignore it as we only want tic binary
RUN cd /unibench/ncurses-6.1 &&\
    scan-build-6.0 -o /report/ncurses ./configure --disable-shared &&\
    (scan-build-6.0 -o /report/ncurses make -j || true) &&\
    cp progs/tic /d/p/clang6/infotocap &&\
    make clean

RUN cd /unibench/jq-1.5 &&\
    scan-build-6.0 -o /report/jq ./configure --disable-shared &&\
    scan-build-6.0 -o /report/jq make -j &&\
    cp jq /d/p/clang6/ &&\
    make clean

RUN cd /unibench/mujs-1.0.2 &&\
    build=debug scan-build-6.0 -o /report/mujs make -j &&\
    cp build/debug/mujs /d/p/clang6/ &&\
    make clean

RUN cd /unibench/xpdf-4.00 &&\
    scan-build-6.0 -o /report/xpdf cmake . &&\
    scan-build-6.0 -o /report/xpdf make -j &&\
    cp xpdf/pdftotext /d/p/clang6/ &&\
    make clean

#--disable-amalgamation can be used for coverage build
RUN apt install -y tcl-dev &&\
    cd /unibench/SQLite-3.8.9 &&\
    scan-build-6.0 -o /report/sqlite ./configure --disable-shared &&\ 
    scan-build-6.0 -o /report/sqlite make -j &&\
    cp sqlite3 /d/p/clang6/ &&\
    make clean

RUN cd /unibench/binutils-5279478 &&\
    scan-build-6.0 -o /report/binutils5279478 ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; scan-build-6.0 -o /report/binutils5279478 ./configure --disable-shared && scan-build-6.0 -o /report/binutils5279478 make -j; cd ..; done  &&\
    cd binutils  &&\
    scan-build-6.0 -o /report/binutils5279478 ./configure --disable-shared &&\
    scan-build-6.0 -o /report/binutils5279478 make nm-new &&\
    cp nm-new /d/p/clang6/nm &&\
    cd /unibench/binutils-5279478 &&\
    make distclean

RUN cd /unibench/binutils-2.28 &&\
    scan-build-6.0 -o /report/binutils2.28 ./configure --disable-shared &&\
    scan-build-6.0 -o /report/binutils2.28 make -j &&\
    cp binutils/objdump /d/p/clang6/ &&\
    make clean

RUN cd /unibench/libpcap-1.8.1 &&\
    scan-build-6.0 -o /report/tcpdump ./configure --disable-shared &&\
    scan-build-6.0 -o /report/tcpdump make -j &&\
    cd /unibench/tcpdump-4.8.1 &&\
    scan-build-6.0 -o /report/tcpdump ./configure &&\
    scan-build-6.0 -o /report/tcpdump make -j &&\
    cp tcpdump /d/p/clang6/ &&\
    make clean &&\
    cd /unibench/libpcap-1.8.1 &&\
    make clean


RUN ls -alh /d/p/*