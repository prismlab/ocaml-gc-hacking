# Ripping out the GC

## Build

```bash
git clone --recurse-submodules https://github.com/prismlab/ocaml-gc-hacking
#Build the unmodified compiler
cd ocaml-gc-hacking/ocaml-4.14-unmodified
./configure --prefix=`pwd`/_install
make -j
make install
#Build the modified compiler
cd ../ocaml-4.14-hacked-gc
make -C runtime -j ocamlrun ocamlrund
#Run tests
cd ../tests
make
```

## Repositories

* `ocaml-4.14-unmodified` -- The unmodified OCaml 4.14 compiler. Used for
    compiling OCaml programs.
* `ocaml-4.14-hacked-gc` -- The hacked OCaml 4.14 compiler. This contains the
    modified `ocamlrun` used to run the modified GC.
* `tests` -- contains some tests
