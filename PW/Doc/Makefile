VERSION := $(shell awk -F\' '/version_number/{print $$2}' ../../include/qe_version.h )
HELPDOC = ../../dev-tools/helpdoc -version $(VERSION)
LATEX   = pdflatex
LATEX2HTML = latex2html

PDFS = user_guide.pdf
AUXS = $(PDFS:.pdf=.aux)
LOGS = $(PDFS:.pdf=.log)
OUTS = $(PDFS:.pdf=.out)
TOCS = $(PDFS:.pdf=.toc)

HTMLS = INPUT_PW.html INPUT_OSCDFT.html
XMLS = $(HTMLS:.html=.xml)
TXTS = $(HTMLS:.html=.txt)

doc:  all
all:  defs pdf html 
pdf: $(PDFS)
html: user_guide

$(PDFS): %.pdf: %.tex
	$(LATEX)  $<
	$(LATEX)  $<

clean:
	- rm -f $(PDFS) $(AUXS) $(LOGS) $(OUTS) $(TOCS) *~
	- rm -rf user_guide/
	- rm -rf input_xx.xsl
	- rm -f $(HTMLS) $(XMLS) $(TXTS)
	- rm -f $(patsubst %,../../Doc/%,$(HTMLS) $(XMLS) $(TXTS))

user_guide: user_guide.pdf
	- rm -rf user_guide/
	latex2html \
                -t "User's Guide for the PWscf package" \
                -html_version 3.2,math \
                -toc_depth 5 -split 5 -toc_stars -show_section_numbers \
                -local_icons -image_type png \
                user_guide.tex
	cd user_guide; \
	for file in *.html; do \
                cp $$file /tmp/$$file; \
                cat /tmp/$$file | sed 's/HREF="http/NAME="http/g' | sed 's/mathend000#//g' - > $$file; \
                rm -f /tmp/$$file; \
        done
	@echo ""
	@echo "***"
	@echo "*** User's Guide created in user_guide/user_guide.html"
	@echo "***"
	@echo ""


defs: input_xx.xsl INPUT_PW.html INPUT_PW.txt INPUT_OSCDFT.html INPUT_OSCDFT.txt link_on_main_doc

input_xx.xsl:
	@(if test ! -f input_xx.xsl; then \
	(if test -f ../../dev-tools/input_xx.xsl; then \
	(ln -sf ../../dev-tools/input_xx.xsl input_xx.xsl) ; \
	else \
	echo ; \
	echo "  Sorry, can not find input_xx.xsl html style file !!!" ; \
	echo ; exit 1; \
	fi) ; fi)

INPUT_PW.html: %.html: %.def input_xx.xsl
	$(HELPDOC) $<
INPUT_PW.txt: %.txt: %.def input_xx.xsl
	$(HELPDOC) $<

INPUT_OSCDFT.html: %.html: %.def input_xx.xsl
	$(HELPDOC) $<
INPUT_OSCDFT.txt: %.txt: %.def input_xx.xsl
	$(HELPDOC) $<

link_on_main_doc:
	-@( cd ../../Doc ; for file in $(HTMLS) $(XMLS) $(TXTS); do ln -fs ../PW/Doc/$$file . ; done )
