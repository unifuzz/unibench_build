FROM unifuzz/unibench:angora_step1

RUN cd /unibench/jq-1.5 && ./configure --disable-shared &&\
    make -j &&\
    cp jq /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp jq /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/mujs-1.0.2 &&\
    build=debug make -j &&\
    cp build/debug/mujs /d/p/angora/fast/ &&\
    make clean && sleep 3 && USE_TRACK=1 build=debug make -j &&\
    cp build/debug/mujs /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/xpdf-4.00 && cmake . &&\
    make -j &&\
    cp xpdf/pdftotext /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 make -j &&\
    cp xpdf/pdftotext /d/p/angora/taint/ &&\
    make clean

#--disable-amalgamation can be used for coverage build
RUN cd /unibench/SQLite-3.8.9 && ./configure --disable-shared &&\ 
    make -j &&\
    cp sqlite3 /d/p/angora/fast/ &&\
    make clean && USE_TRACK=1 ASAN_OPTIONS="detect_leaks=0" make -j &&\
    cp sqlite3 /d/p/angora/taint/ &&\
    make clean

RUN cd /unibench/binutils-5279478 &&\
    ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; ./configure --disable-shared && make -j; cd ..; done  &&\
    cd binutils  &&\
    ./configure --disable-shared &&\
    make nm-new &&\
    cp nm-new /d/p/angora/fast/nm &&\
    cd /unibench/binutils-5279478 &&\
    for i in bfd libiberty opcodes libctf; do cd $i; make clean && USE_TRACK=1 make -j; cd ..; done  &&\
    cd binutils  && make clean &&\
    USE_TRACK=1 make nm-new &&\
    cp nm-new /d/p/angora/taint/nm &&\
    cd .. && make distclean

RUN ls -alh /d/p/angora/*