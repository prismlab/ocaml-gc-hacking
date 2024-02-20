%.byte: %.ml
	@echo "Building: " $@
	@../ocaml-4.14-unchanged/_install/bin/ocamlc -custom -cclib -L../ocaml-4.14-hacked-gc/runtime/verified_gc -cclib -lvergc -cclib -lrust_allocator -o $@ $<

benchmark: binarytrees.csv fasta.csv quicksort.csv fannkuchredux.csv

test: binarytrees.byte fasta.byte quicksort.byte fannkuchredux.byte count_change.byte
	@echo "Running binarytrees"
	MIN_EXPANSION_WORDSIZE=134217728 '../ocaml-4.14-hacked-gc/runtime/ocamlrun' binarytrees.byte 16
	@echo "Running fasta"
	MIN_EXPANSION_WORDSIZE=134217728 '../ocaml-4.14-hacked-gc/runtime/ocamlrun' fasta.byte 2_500_000A
	@echo "Running quicksort"
	MIN_EXPANSION_WORDSIZE=8000000 '../ocaml-4.14-hacked-gc/runtime/ocamlrun' quicksort.byte 5_000_000
	@echo "Running fannkuchredux"
	'../ocaml-4.14-hacked-gc/runtime/ocamlrun' fannkuchredux.byte 11
	@echo "Running count_change"
	MIN_EXPANSION_WORDSIZE=20000000 ../ocaml-4.14-hacked-gc/runtime/ocamlrun ./count_change.byte 150
	@echo "Running nbodies"

count_change.csv: count_change.byte
	MIN_EXPANSION_WORDSIZE=20000000 hyperfine -P param 100 150 -D 10 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../ocaml-4.14-bdwgc/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'

binarytrees.csv: binarytrees.byte
	MIN_EXPANSION_WORDSIZE=134217728 hyperfine -P depth 11 16  --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {depth}' '../ocaml-4.14-bdwgc/runtime/ocamlrun $< {depth}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {depth}'

fasta.csv: fasta.byte
	MIN_EXPANSION_WORDSIZE=134217728 hyperfine -P param 5_00_000 2_500_000 -D 5_00_000 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../ocaml-4.14-bdwgc/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'

quicksort.csv: quicksort.byte
	MIN_EXPANSION_WORDSIZE=8000000 hyperfine -P param 1_000_000 5_000_000 -D 1_000_000 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../ocaml-4.14-bdwgc/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'

fannkuchredux.csv: fannkuchredux.byte
	hyperfine -P param 8 11 -D 1 --export-csv $@ '../ocaml-4.14-hacked-gc/runtime/ocamlrun $< {param}' '../ocaml-4.14-bdwgc/runtime/ocamlrun $< {param}' '../ocaml-4.14-unchanged/runtime/ocamlrun $< {param}'

HACKED_GC=../ocaml-4.14-hacked-gc/runtime/ocamlrun
BDWGC=../ocaml-4.14-bdwgc/runtime/ocamlrun
UNCHANGED=../ocaml-4.14-unchanged/runtime/ocamlrun

bench: binarytrees.byte fasta.byte quicksort.byte fannkuchredux.byte count_change.byte
	MIN_EXPANSION_WORDSIZE=100000000 hyperfine --export-csv \
		# binarytrees
		'$(HACKED_GC) binarytrees.byte 16' \
		'$(BDWGC) binarytrees.byte 16' \
		'$(UNCHANGED) binarytrees.byte 16' \
		# fasta
		'$(HACKED_GC) fasta.byte 5_000_000' \
		'$(BDWGC) fasta.byte 5_000_000' \
		'$(UNCHANGED) fasta.byte 5_000_000' \
		# count_change
		'$(HACKED_GC) count_change.byte 200' \
		'$(BDWGC) count_change.byte 200' \
		'$(UNCHANGED) count_change.byte 200'
		# fannkuchredux
		'$(HACKED_GC) fannkuchredux.byte 10' \
		'$(BDWGC) fannkuchredux.byte 10' \
		'$(UNCHANGED) fannkuchredux.byte 10' \
		# quicksort
		'$(HACKED_GC) quicksort.byte 2_500_000' \
		'$(BDWGC) quicksort.byte 2_500_000' \
		'$(UNCHANGED) quicksort.byte 2_500_000'

cpdf-src:
	git clone https://github.com/johnwhitington/cpdf-source $@
	cd $@; git checkout v2.6.2

cpdf-src-clean:
	rm -rf cpdf-src

cpdf: cpdf-src
	PWD=$(shell pwd)
	BCSUFFIX='-vergc' ALL_LDFLAGS='-use-runtime $(PWD)/../ocaml-4.14-hacked-gc/runtime/ocamlrun' $(MAKE) -C $< bc
	@echo "Running cpdf with verified gc runtime"
	MIN_EXPANSION_WORDSIZE=134217728 cpdf-src/cpdf-vergc -merge $</*.pdf -o /dev/null

clean:
	rm -f *.cmi *.cmo *.cmx *.o *.byte
