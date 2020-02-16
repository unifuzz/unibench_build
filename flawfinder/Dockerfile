FROM unifuzz/unibench:gcc
RUN pip install flawfinder
RUN mkdir /report &&\
    cd /unibench &&\
    for i in */; do flawfinder --html $i > /report/${i%/*}.html; done
