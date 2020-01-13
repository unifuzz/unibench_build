FROM unifuzz/unibench:gcc AS gcc

FROM zjuchenyuan/vuzzer64
COPY --from=gcc /d/p/normal /d/p/normal
ADD names /d/p/vbin/names
ADD pkl /d/p/vbin/pkl

RUN apt update && apt install -y libpng-dev libtiff-dev libjpeg-dev libjbig-dev