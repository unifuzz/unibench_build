# unibench_build
Build Docker images for unibench

## Images

[unifuzz/unibench](https://hub.docker.com/r/unifuzz/unibench/tags)

Take **exiv2** as an example to illustrate the binary path:

### unifuzz/unibench:gcc

Binary Path: **/d/p/normal/exiv2**

This image is built using gcc, used for build Vuzzer and QSYM images.

### unifuzz/unibench:afl

Binary Path: **/d/p/justafl/exiv2** and **/d/p/aflasan/exiv2**

This image is built using afl-gcc, including `justafl` and `aflasan` (AFL_USE_ASAN=1 make).

justafl binaries are used for afl-based fuzzers, like AFL, AFLFast, MOPT, T-Fuzz.

aflasan binaries are used for QSYM.

### unifuzz/unibench:aflfast

Binary Path: **/d/p/justafl/exiv2**

Just a copy of afl built binaries.

## unifuzz/unibench:angora

Binary Path: **/d/p/angora/fast/exiv2** and **/d/p/angora/taint/exiv2**, both two binaries are required by Angora.

### unifuzz/unibench:honggfuzz

Binary Path: **/d/p/honggfuzz/exiv2**

### unifuzz/unibench:vuzzer

Binary Path: **/d/p/normal/exiv2**

Contain names and pkl files:

```
/d/p/vbin/names/exiv2.names
/d/p/vbin/pkl/exiv2.pkl
```

### unifuzz/unibench:coverage

Binary Path: **/d/p/cov/exiv2**

Source Code Folder: **/unibench/exiv2-0.26** (coverage info contained)

Used for calculate coverage info.

The folder can not be moved or renamed, as coverage info contain absolute path.

## Verify correct build

Here are some commands to verify these docker images are built correctly, for example, ldd a aflasan binary should output libasan.so.

```
# ldd should not report something as not found
$ docker run -it --rm unifuzz/unibench:gcc ldd /d/p/normal/exiv2
        linux-vdso.so.1 =>  (0x00007ffff7ffa000)
        libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007ffff7bd3000)
        libexpat.so.1 => /lib/x86_64-linux-gnu/libexpat.so.1 (0x00007ffff79aa000)
        libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007ffff7790000)
        libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007ffff740e000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007ffff7105000)
        libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007ffff6eef000)
        libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007ffff6cd2000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007ffff6908000)
        /lib64/ld-linux-x86-64.so.2 (0x00007ffff7dd7000)

# gcc build should not have afl strings
$ docker run -it --rm unifuzz/unibench:gcc strings /d/p/normal/exiv2 |grep afl|wc -l
0

# afl build should have some afl strings
$ docker run -it --rm unifuzz/unibench:afl strings /d/p/justafl/exiv2 |grep afl|head
__afl_global_area_ptr
__afl_maybe_log
__afl_area_ptr
__afl_setup
__afl_store
__afl_prev_loc
__afl_return
__afl_setup_failure
__afl_setup_first
__afl_setup_abort

# aflasan build should link to libasan.so
$ docker run -it --rm unifuzz/unibench:afl ldd /d/p/aflasan/exiv2 |grep asan
        libasan.so.2 => /usr/lib/x86_64-linux-gnu/libasan.so.2 (0x00007ffff6e6a000)

# angora fast build should contain angora string, and should not contain DataFlow Sanitizer functions
$ docker run -it --rm unifuzz/unibench:angora strings /d/p/angora/fast/exiv2 | grep angora | head -1
_ZN13angora_common4defs23DISABLE_CPU_BINDING_VAR17h3d247f62d1f76855E

$ docker run -it --rm unifuzz/unibench:angora strings /d/p/angora/fast/exiv2 | grep 'dfs\$' | wc -l
0

# angora taint build should contain many DataFlow Sanitizer functions
$ docker run -it --rm unifuzz/unibench:angora strings /d/p/angora/taint/exiv2 |grep 'dfs\$'|wc -l
10566

# honggfuzz build should have hfuzz strings
$ docker run -it --rm unifuzz/unibench:honggfuzz strings /d/p/honggfuzz/exiv2 |grep hfuzz|head -1
hfuzz_trace_cmp8

# vuzzer image should contain some pkl and names files, loadable by python pickle
$ docker run -it --rm unifuzz/unibench:vuzzer python -c 'import pickle; print(pickle.load(open("/d/p/vbin/names/exiv2.names","rb")))'
[set(['colr', ' o', '\x02\x00\x00', '\x00\xf8\xff', '\x08\x00', '(\x00', ' @', '\x03o', 'uuid', '\x04\x00\x00', '\x00\xfeQ', '\x00\xfeP', '\x00\xfeT', '\x10\x00', '\x03~', '\x03}', '+\xff', '\x04\x04', '\x01\xff', '\x00\xff\xfe', '\x00\xff\xff', '\x00\xff\xfd', '\x00\xff\xf7', ' (', ' )', '\x01\x8f', '\x00\xff\xef', '\x01\x00\x04', '\x00\xd7\xff', ' :', ' 9', ' >', ' \x00', ' \x01', ' \x0b', ' \r', '\x83\xbb', ' \x17', ' \x15', ' \x1a', '\x08c', ' \x18', ' \x1e', ' \x1f', ' \x1c', '\x06\x1b', '0\x1e', '0\x1f', '0\x1c', '0\x1d', '0\x00', '0\x01', '0\x07', '0\n', '0\x08', '0\x0e', '0\x0f', '0\x0c', '\x00\xdf\xff', ';\x9a\xc9\xff', '\x00\xff\x1b', '\x00\xab', '\x00\xa8', '\x03\xe7', '0?', '\x00\xff\x0c', '\x00\xb7', '\x00\xbb', '\x00\xbf', '\x01\x00', '@\x00', '\x0e\xff\xff', '\x00\xc7', '\x00\xc4', '\x00\xc0', '\x00\xcf', '\x10\xff\xff', '\x00\xd6', '\x00\xd7', '\x00\xda', '\x00\xdb', '\x00\xffd', '\x00\xd9', '\x00\xfd\xe8', '\x00\xdf', '\x00\xdd', '\x00\xe2', 'ihdr', '\x00\xe0', '\x00\xe1', '\x01+', '\x00\xef', '\x00\xed', '\x00\xf0', '\x00\xf6', '\x00\xf7', '\x01G\xae\x14', '\x00\xf8', '\x00\xf9', '\x00\xfe', '\x00\xff', '\x00\xfc', '\x00\xfd', '\x01\x00\x08', '\x01\x00\t', '\x01\x00\x02', '\x01\x00\x03', '\x01\x00\x00', '\x01\x00\x01', '\x01\x00\x06', '\x01\x00\x07', '\x04\x00', '\x01\x00\x05', '\x03\x00\x00', '\x00\xd8', '\x00\xff\xff\xff\xff\xff\xff\xff\x80', '\x00\xff\xff\xff\xff\xff\xff\xff\x81', '/\xef', '\x00\xfd\xef', '\x04$', '\x04"', '\x03F\xdc', '\x02\xbc', '\x1f\xa3', '\x01\xf4', '\x01J', '\x92|', '\x01\xff\xfe', '\x00\xa0\x00', '\x01\x00\x00\x00', '\x00\xff\xff\xff\xff\xff\xff\xff\xfe', '\x00\xff\xff\xff\xff\xff\xff\xff\xff', 'SR', '\x02\xff', '\x7f\xff', '\x80\x00', '\x80\x00\x00\x00', '\x05]', '\x00\xff\xff\xff\xff', '\x0f\xff\xff\xff', '\x00\xff\xff\xff\xfb', '\x87s', '\x07\xff', '\x06\x0c', '\x00\xdb\xff', '!\x8f', '\x1f\xff\xff\xff', '\x92\x86', '\x18\x00', '\x87i', '\x03\x00', '\x1f\xff', '\x00\xff\xff\xfc|', '\x00\xfd\xcf', 'jp2h', 'jp2c', '\x00\xff\xff\xfc\x19']), set(['\x00', '\x83', '\x04', '\x87', '\x08', '\x0c', '\x8f', '\x10', '\x14', '\x18', '\x1c', ' ', '\xa3', '$', '(', '\xab', ',', '0', '4', '\xb7', '8', '\xbb', '<', '\xbf', '@', '\xc7', 'L', '\xcf', 'P', 'T', '\xd7', 'X', '\xdb', '\\', '\xdf', '`', 'd', '\xe7', 'h', 'l', '\xef', 'p', 't', '\xf7', '\xfb', '|', '\xff', '\x80', '\x03', '\x07', '\x0b', '\x0f', '\x90', '\x13', '\x94', '\x17', '\x1b', '\x1f', '\xa0', '#', "'", '\xa8', '+', '/', ';', '\xbc', '?', '\xc0', 'C', '\xc4', 'G', 'K', 'O', 'S', 'W', '\xd8', '[', '\xdc', '_', '\xe0', 'c', 'g', '\xe8', 'o', '\xf0', 's', '\xf4', '\xf8', '\xfc', '\x7f', '\x81', '\x02', '\x06', '\n', '\x0e', '\x12', '\x16', '\x1a', '\x1e', '"', '&', '*', '.', '2', '6', ':', '>', 'F', '\xc9', 'J', 'N', 'R', '\xd9', 'Z', '\xdd', '\xe1', 'f', 'j', '\xed', 'n', 'r', '\xf9', 'z', '\xfd', '~', '\x01', '\x05', '\x86', '\t', '\r', '\x11', '\x92', '\x15', '\x19', '\x9a', '\x1d', '!', '%', ')', '-', '\xae', '5', '9', '=', 'A', 'E', 'I', 'M', 'Q', '\xd6', 'Y', '\xda', ']', '\xe2', 'i', 'm', 'u', '\xf6', 'y', '}', '\xfe'])]

# coverage build should contain gcno files in code folder
$ docker run -it --rm unifuzz/unibench:coverage find /unibench/exiv2-0.26 -type f -name '*.gcno'|head
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XMPMeta-GetSet.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XMPMeta-Serialize.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XML_Node.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/WXMPIterator.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/ParseRDF.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XMPMeta-Parse.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/MD5.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/WXMPUtils.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XMPCore_Impl.cpp.gcno
/unibench/exiv2-0.26/xmpsdk/CMakeFiles/xmp.dir/src/XMPUtils-FileInfo.cpp.gcno
```
