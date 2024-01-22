%.byte: %.ml
	@echo "Building: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -custom -cclib -L../ocaml-4.14-hacked-gc/runtime/verified_gc -cclib -lvergc -cclib -lrust_allocator -o $@ $<


binarytrees.csv: binarytrees.byte
	MIN_EXPANSION_WORDSIZE=134217728 hyperfine -P depth 11 16  --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {depth}' '../bdwgc-branch/runtime/ocamlrun $< {depth}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {depth}'

fasta.csv: fasta.byte
	MIN_EXPANSION_WORDSIZE=134217728 hyperfine -P param 5_00_000 2_500_000 -D 5_00_000 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../bdwgc-branch/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'

quicksort.csv: quicksort.byte
	MIN_EXPANSION_WORDSIZE=8000000 hyperfine -P param 1_000_000 5_000_000 -D 1_000_000 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../bdwgc-branch/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'


clean:
	rm -f *.cmi *.cmo *.cmx *.o *.byte
