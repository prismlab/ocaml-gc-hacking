all: test1.byte binarytrees.byte

DEBUG=false
%.byte: %.ml
	@echo "Building: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -o $@ $<
	@echo "Running: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -o $@ $<
	@if [ $(DEBUG) = "true" ]; then \
		../ocaml-4.14-hacked-gc/runtime/ocamlrund $@; \
	else \
		../ocaml-4.14-hacked-gc/runtime/ocamlrun $@; \
	fi

clean:
	rm -f *.cmi *.cmo *.cmx *.o *.byte
