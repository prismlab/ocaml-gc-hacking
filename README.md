# Ripping out the GC

## Build

```bash
git clone --recurse-submodules https://github.com/prismlab/ocaml-gc-hacking
#Build the unchanged compiler
cd ocaml-gc-hacking/ocaml-4.14-unchanged
./configure --prefix=`pwd`/_install  --disable-naked-pointers
make -j
make install
#Build the modified compiler
cd ../ocaml-4.14-hacked-gc
./configure --disable-naked-pointers
make -C runtime -j ocamlrun ocamlrund
#Run tests
cd ../tests
make
```

## Checkout bdwgc branch

Need to have bdwgc installed globally

```bash
git worktree add ../bdwgc-branch origin/bdwgc
```

Run the same build steps as hacked-gc


## Running binarytrees.ml

``` bash
MIN_EXPANSION_WORDSIZE=2097152 HEIGHT=10 make -B binarytrees.byte
```

- `MIN_EXPANSION_WORDSIZE` controls the size of heap chunk allocated by the allocator. The size of heap will be `MIN_EXPANSION_WORDSIZE * <word-size>`.
- `HEIGHT` is the parameter used in binarytrees.ml


## Repositories

* `ocaml-4.14-unchanged` -- The unchanged OCaml 4.14 compiler. Used for
    compiling OCaml programs.
* `ocaml-4.14-hacked-gc` -- The hacked OCaml 4.14 compiler. This contains the
    modified `ocamlrun` used to run the modified GC. There are 3 modified
    branches:
    + `malloc-nogc` -- uses `malloc` and does not GC
    + `bdwgc` -- uses Boehm GC
    + `verified-gc` -- uses the verified GC (+ unverified allocator). This is the branch that's pointed to by default in the submodule.
* `tests` -- contains some test
