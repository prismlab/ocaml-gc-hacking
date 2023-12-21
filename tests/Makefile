all: test1.byte

test1.byte: test1.ml
	../ocaml-4.14-unchanged/_install/bin/ocamlc -o test1.byte test1.ml
	../ocaml-4.14-hacked-gc/runtime/ocamlrund test1.byte

clean:
	rm -f *.cmi *.cmo *.cmx *.o *.byte

EXE=
debug:
	../ocaml-4.14-hacked-gc/runtime/ocamlrund $(EXE)
