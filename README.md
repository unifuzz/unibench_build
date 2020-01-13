# unibench_build
Build Docker images for unibench

## Images

[unifuzz/unibench](https://hub.docker.com/r/unifuzz/unibench/tags)

Take **exiv2** as an example to give the binary path:

### unifuzz/unibench:gcc

/d/p/normal/exiv2

This image is built using gcc, used for build Vuzzer and QSYM images.

### unifuzz/unibench:afl

/d/p/justafl/exiv2

/d/p/aflasan/exiv2

This image is built using afl-gcc, including justafl and aflasan (AFL_USE_ASAN=1 make).

justafl binaries are used for afl-based fuzzers, like AFL, AFLFast, MOPT, T-Fuzz.

aflasan binaries are used for QSYM.

### unifuzz/unibench:honggfuzz

/d/p/honggfuzz/exiv2

### unifuzz/unibench:coverage

/d/p/cov/exiv2

Used for calculate coverage info, `/unibench/exiv2-0.26` folder. 

Note: The folder can not be moved or renamed, as coverage info contain absolute path.

## Verify correct build

Here are some commands to verify these docker images are built correctly, for example, ldd a aflasan binary should output libasan.so.

```
# docker run -it --rm unifuzz/unibench:gcc ldd /d/p/normal/exiv2
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

# docker run -it --rm unifuzz/unibench:gcc strings /d/p/normal/exiv2 |grep afl|wc -l
0

# docker run -it --rm unifuzz/unibench:afl strings /d/p/justafl/exiv2 |grep afl|head
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

# docker run -it --rm unifuzz/unibench:afl ldd /d/p/aflasan/exiv2 |grep asan
        libasan.so.2 => /usr/lib/x86_64-linux-gnu/libasan.so.2 (0x00007ffff6e6a000)

# docker run -it --rm unifuzz/unibench:honggfuzz strings /d/p/honggfuzz/exiv2 |grep hfuzz|head -1
hfuzz_trace_cmp8

# docker run -it --rm unifuzz/unibench:coverage find /unibench/exiv2-0.26 -type f -name '*.gcno'|head
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