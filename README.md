# Ripping out the GC

## Reps

* `ocaml-4.14-unmodified` -- The unmodified OCaml 4.14 compiler. Used for
    compiling OCaml programs.
* `ocaml-4.14-hacked-gc` -- The hacked OCaml 4.14 compiler. This contains the
    modified `ocamlrun` used to run the modified GC.

### Building the hacked GC with Boehm GC

On macOS,

```bash
brew install bdw-gc
CFLAGS="-I/opt/homebrew/Cellar/bdw-gc/8.2.4/include -L/opt/homebrew/Cellar/bdw-gc/8.2.4/lib/ -lgc" ./configure
```

on Ubuntu,
```
sudo apt install libgc-dev
LIBS="-lgc" ./configure
```
