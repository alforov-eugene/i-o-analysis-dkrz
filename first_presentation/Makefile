.PHONY: default, pdf, open, clean

name=presentation
sheet=main

default: open

pdf:
	rm -f $(name).pdf
	pdflatex $(sheet).tex
	mv $(sheet).pdf $(name).pdf

open: pdf clean
	xdg-open $(name).pdf

clean:
	rm -f *.zip
	rm -f *.fls
	rm -f *.log
	rm -f *.out
	rm -f *.aux
	rm -f *.fdb*
	rm -f *.toc
	rm -f *.snm
	rm -f *.nav
	rm -f *.vrb
