FROM unifuzz/unibench:gcc
RUN wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/rough-auditing-tool-for-security/rats-2.4.tgz &&\
    tar xf rats-2.4.tgz &&\
    cd rats-2.4 &&\
    ./configure &&\
    make && make install

RUN mkdir /report &&\
    cd /unibench &&\
    for i in */; do rats --html $i > /report/${i%/*}.html; done