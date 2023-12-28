# Ripping out the GC

## Build

```bash
git clone --recurse-submodules https://github.com/prismlab/ocaml-gc-hacking
#Build the unchanged compiler
cd ocaml-gc-hacking/ocaml-4.14-unchanged
./configure --prefix=`pwd`/_install
make -j
make install
#Build the modified compiler
cd ../ocaml-4.14-hacked-gc
./configure
make -C runtime -j ocamlrun ocamlrund
#Run tests
cd ../tests
make
```

## Repositories

* `ocaml-4.14-unchanged` -- The unchanged OCaml 4.14 compiler. Used for
    compiling OCaml programs.
* `ocaml-4.14-hacked-gc` -- The hacked OCaml 4.14 compiler. This contains the
    modified `ocamlrun` used to run the modified GC. There are 3 modified
    branches:
    + `malloc-nogc` -- uses `malloc` and does not GC
    + `bdwgc` -- uses Boehm GC
    + `verified-gc` -- uses the verified GC (+ unverified allocator)
* `tests` -- contains some tests
