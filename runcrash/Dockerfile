# This image is used to run crash and save to database
# So, exploitable and python pymysql are installed
FROM unifuzz/unibench:qsym
RUN apt update && apt install -y python3-pip gdb htop vim nano &&\
    pip3 install pymysql &&\
    git clone https://github.com/jfoote/exploitable &&\
    cd exploitable &&\
    python setup.py install &&\
    echo -e "set pagination off\nset confirm off\nsource /usr/local/lib/python2.7/dist-packages/exploitable-1.32-py2.7.egg/exploitable/exploitable.py" >> ~/.gdbinit