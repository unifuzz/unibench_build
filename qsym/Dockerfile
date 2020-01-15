FROM zjuchenyuan/qsym
COPY --from=unifuzz/unibench:afl /d/p/justafl /d/p/justafl
COPY --from=unifuzz/unibench:afl /d/p/aflasan /d/p/aflasan

RUN apt update && apt install -y libpng-dev libtiff-dev libjpeg-dev libjbig-dev