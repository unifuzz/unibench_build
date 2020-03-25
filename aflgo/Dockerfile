FROM zjuchenyuan/aflgo
RUN git clone https://github.com/UNIFUZZ/unibench /unibench &&\
    cd /unibench && \
    mkdir mp3gain-1.5.2 && cd mp3gain-1.5.2 && mv ../mp3gain-1.5.2.zip ./ && unzip -q mp3gain-1.5.2.zip && rm mp3gain-1.5.2.zip && cd .. &&\
    ls *.zip|xargs -i unzip -q '{}' &&\
    ls *.tar.gz|xargs -i tar xf '{}' &&\
    rm -r .git/ *.tar.gz *.zip &&\
    mv SQLite-8a8ffc86 SQLite-3.8.9 && mv binutils_5279478 binutils-5279478 && mv libtiff-Release-v3-9-7 libtiff-3.9.7 &&\
    ls -alh
RUN mkdir -p /d/p/aflgo

ENV CC=/aflgo/afl-clang-fast CXX=/aflgo/afl-clang-fast++

ADD targets /targets

ENV SUBJECT=/unibench/Bento4-1.5.1-628 TMP_DIR=/unibench/Bento4-1.5.1-628/obj-aflgo/temp \
    ADDITIONAL="-targets=/unibench/Bento4-1.5.1-628/obj-aflgo/temp/BBtargets.txt -outdir=/unibench/Bento4-1.5.1-628/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=/unibench/Bento4-1.5.1-628/obj-aflgo/temp/distance.cfg.txt" \
    BIN=mp42aac BINPATH=mp42aac

RUN mkdir -p $TMP_DIR && cp /targets/bento4.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" cmake . &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt &&\
    mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt &&\
    cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt &&\
    mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt &&\
    $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    rm -r CMakeCache.txt CMakeFiles/ &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" cmake . &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/exiv2-0.26 TMP_DIR=/unibench/exiv2-0.26/obj-aflgo/temp \
    ADDITIONAL="-targets=/unibench/exiv2-0.26/obj-aflgo/temp/BBtargets.txt -outdir=/unibench/exiv2-0.26/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=/unibench/exiv2-0.26/obj-aflgo/temp/distance.cfg.txt" \
    BIN=exiv2 BINPATH=bin/exiv2

RUN mkdir -p $TMP_DIR && cp /targets/exiv2.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" cmake -DEXIV2_ENABLE_SHARED=OFF . &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\ 
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt &&\
    mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt &&\
    cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt &&\
    mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt &&\
    $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean

RUN cd $SUBJECT &&\
    rm -r CMakeCache.txt CMakeFiles/ &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" cmake -DEXIV2_ENABLE_SHARED=OFF . &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/gdk-pixbuf-2.31.1

ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt" \
    BIN=gdk-pixbuf-pixdata BINPATH=gdk-pixbuf/gdk-pixbuf-pixdata

RUN apt update && apt install -y libglib2.0-dev gtk-doc-tools libtiff-dev libpng-dev &&\
    mkdir -p $TMP_DIR && cp /targets/gdk-pixbuf.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./autogen.sh --enable-static=yes --enable-shared=no --with-included-loaders=yes &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt &&\
    mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt &&\
    cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt &&\
    mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt &&\
    $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean

RUN cd $SUBJECT &&\
    rm configure &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./autogen.sh --enable-static=yes --enable-shared=no --with-included-loaders=yes &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/jasper-2.0.12 \
    BIN=imginfo BINPATH=src/appl/imginfo
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/jasper.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" cmake -DJAS_ENABLE_SHARED=OFF -DALLOW_IN_SOURCE_BUILD=ON . &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean && rm -r CMakeCache.txt CMakeFiles/
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" cmake -DJAS_ENABLE_SHARED=OFF -DALLOW_IN_SOURCE_BUILD=ON . &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/jhead-3.00 \
    BIN=jhead BINPATH=jhead
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/jhead.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    $CC $ADDITIONAL ./jhead.c ./jpgfile.c ./jpgqguess.c ./paths.c ./exif.c ./iptc.c ./gpsinfo.c ./makernote.c -lm -o jhead &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    $CC $ADDITIONAL2 ./jhead.c ./jpgfile.c ./jpgqguess.c ./paths.c ./exif.c ./iptc.c ./gpsinfo.c ./makernote.c -lm -o jhead &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/libtiff-3.9.7 \
    BIN=tiffsplit BINPATH=tools/tiffsplit
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/libtiff.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./autogen.sh && CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean && rm configure
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./autogen.sh && CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/lame-3.99.5 \
    BIN=lame BINPATH=frontend/lame
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/lame.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/mp3gain-1.5.2 \
    BIN=mp3gain BINPATH=mp3gain
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/mp3gain.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    sed -i 's/CC=/CC?=/' Makefile &&\
    CC="$CC $ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CC="$CC $ADDITIONAL2"  make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/swftools-0.9.2 \
    BIN=wav2swf BINPATH=src/wav2swf
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/swftools.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CC="$CC $ADDITIONAL" ./configure &&\
    CC="$CC $ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CC="$CC $ADDITIONAL2" ./configure  &&\
    CC="$CC $ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/flvmeta-1.2.1 \
    BIN=flvmeta BINPATH=src/flvmeta
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/flvmeta.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" cmake . &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean && rm -r CMakeCache.txt CMakeFiles/
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" cmake . &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/cflow-1.6 \
    BIN=cflow BINPATH=src/cflow
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/cflow.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/ncurses-6.1 \
    BIN=tic BINPATH=progs/tic
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/ncurses.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo/infotocap

ENV SUBJECT=/unibench/jq-1.5 \
    BIN=jq BINPATH=jq
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/jq.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/xpdf-4.00 \
    BIN=pdftotext BINPATH=xpdf/pdftotext
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/xpdf.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" cmake . &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean && rm -r CMakeCache.txt CMakeFiles/
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" cmake . &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/SQLite-3.8.9  \
    BIN=sqlite3 BINPATH=sqlite3
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/sqlite.txt $TMP_DIR/BBtargets.txt &&\
    apt install -y tcl-dev &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared --disable-amalgamation &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared --disable-amalgamation &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo

ENV SUBJECT=/unibench/binutils-5279478  \
    BIN=nm-new BINPATH=binutils/nm-new
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/binutils5279478.txt $TMP_DIR/BBtargets.txt &&\
    cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared && CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL"  make -j; cd ..; done  &&\
    cd binutils && CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" ./configure --disable-shared && CFLAGS="$ADDITIONAL" CXXFLAGS="$ADDITIONAL" make nm-new && cd .. &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared &&\
    for i in bfd libiberty opcodes libctf; do cd $i; CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared && CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2"  make -j; cd ..; done  &&\
    cd binutils && CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" ./configure --disable-shared && CFLAGS="$ADDITIONAL2" CXXFLAGS="$ADDITIONAL2" make nm-new && cd .. &&\
    cp $BINPATH /d/p/aflgo/nm

ENV SUBJECT=/unibench/binutils-2.28  \
    BIN=objdump BINPATH=binutils/objdump
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/binutils2.28.txt $TMP_DIR/BBtargets.txt &&\
    apt install -y tcl-dev &&\
    cd $SUBJECT &&\
    CC="$CC $ADDITIONAL" CXX="$CXX $ADDITIONAL" ./configure --disable-shared &&\
    CC="$CC $ADDITIONAL" CXX="$CXX $ADDITIONAL" make -j &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT && make distclean &&\
    CC="$CC $ADDITIONAL2" CXX="$CXX $ADDITIONAL2" ./configure --disable-shared &&\
    CC="$CC $ADDITIONAL2" CXX="$CXX $ADDITIONAL2" make -j &&\
    cp $BINPATH /d/p/aflgo


ENV SUBJECT=/unibench/ffmpeg-4.0.1  \
    BIN=ffmpeg_g BINPATH=ffmpeg_g
ENV TMP_DIR=$SUBJECT/obj-aflgo/temp \
    ADDITIONAL="-targets=$SUBJECT/obj-aflgo/temp/BBtargets.txt -outdir=$SUBJECT/obj-aflgo/temp -flto -fuse-ld=gold -Wl,-plugin-opt=save-temps" \
    ADDITIONAL2="-distance=$SUBJECT/obj-aflgo/temp/distance.cfg.txt"
RUN mkdir -p $TMP_DIR && cp /targets/ffmpeg.txt $TMP_DIR/BBtargets.txt &&\
    apt update && apt install -y nasm &&\
    cd $SUBJECT &&\
    ./configure --disable-asm --disable-shared --cc="$CC $ADDITIONAL" --cxx="$CXX $ADDITIONAL"  &&\
    make -j4 &&\
    cat $TMP_DIR/BBnames.txt | rev | cut -d: -f2- | rev | sort | uniq > $TMP_DIR/BBnames2.txt && mv $TMP_DIR/BBnames2.txt $TMP_DIR/BBnames.txt && cat $TMP_DIR/BBcalls.txt | sort | uniq > $TMP_DIR/BBcalls2.txt && mv $TMP_DIR/BBcalls2.txt $TMP_DIR/BBcalls.txt && $AFLGO/scripts/genDistance.sh $SUBJECT $TMP_DIR $BIN &&\
    make clean
RUN cd $SUBJECT &&\
    ./configure --disable-asm --disable-shared --cc="$CC $ADDITIONAL2" --cxx="$CXX $ADDITIONAL2" &&\
    make -j4 &&\
    cp $BINPATH /d/p/aflgo/ffmpeg
    