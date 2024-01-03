HEIGHT?=5
all: test1.byte binarytrees.byte

DEBUG=false
%.byte: %.ml
	@echo "Building: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -custom -cclib -L../ocaml-4.14-hacked-gc/runtime/verified_gc -cclib -lvergc -cclib -lrust_allocator -o $@ $<
	@echo "Running: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -custom -cclib -L../ocaml-4.14-hacked-gc/runtime/verified_gc -cclib -lvergc -cclib -lrust_allocator -o $@ $<
	@if [ $(DEBUG) = "true" ]; then \
		../ocaml-4.14-hacked-gc/runtime/ocamlrund $@ $(HEIGHT); \
	else \
		../ocaml-4.14-hacked-gc/runtime/ocamlrun $@ $(HEIGHT); \
	fi

clean:
	rm -f *.cmi *.cmo *.cmx *.o *.byte
