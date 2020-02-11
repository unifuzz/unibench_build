FROM unifuzz/unibench:gcc
RUN pip install flawfinder
RUN cd /unibench &&\
    for i in */; do flawfinder --html $i > ${i%/*}.html; done