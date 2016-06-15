.PHONY: default, pdf, open, clean

name='i-o-Analysis'

default: open

pdf:
	rm -f $(name).pdf
	pdflatex bericht.tex
	mv bericht.pdf $(name).pdf

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
