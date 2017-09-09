SOURCE := Summit17_logo.tex

INPUTS := $(shell sed -n 's/\ *\\input{\([^}]*\).pdf_tex}.*$$/\1/p' $(SOURCE))
PDF_INPUTS := $(addsuffix .pdf, $(INPUTS))
PDF_TEX_INPUTS := $(addsuffix .pdf_tex, $(INPUTS))
SVG_INPUTS := $(addsuffix .svg, $(INPUTS))

SVG_MOURNING_INPUTS := MourningOfficeBrainz_logo_no_text.svg OfficeMourningBrainz_logo_no_text.svg

FLAGS := alone atoffice mourningbrainz mourningoffice

PDF_TARGETS := $(addprefix $(patsubst %.tex,%-,$(SOURCE)), $(addsuffix .pdf, $(FLAGS)))
ALPHA_PNG_TARGETS := $(patsubst %.pdf,%-alpha.png,$(PDF_TARGETS))
FLATTEN_PNG_TARGETS := $(patsubst %.pdf,%-flatten.png,$(PDF_TARGETS))

INPUT_MB_LOGOS := $(addprefix MetaBrainz_, $(addsuffix .svg, logo logo_no_text logo_short_horizontal logo_short_vertical))
PDF_INPUT_MB_LOGOS := $(patsubst %.svg,%.pdf,$(INPUT_MB_LOGOS))
PDF_TEX_INPUT_MB_LOGOS := $(patsubst %.svg,%.pdf_tex,$(INPUT_MB_LOGOS))
ALPHA_MB_LOGOS := $(patsubst %.svg,%-alpha.png,$(INPUT_MB_LOGOS))
FLATTEN_MB_LOGOS := $(patsubst %.svg,%-flatten.png,$(INPUT_MB_LOGOS))
GREENLESS_MB_LOGOS := $(patsubst %.svg,%-greenless.png,$(INPUT_MB_LOGOS))
ALL_MB_LOGOS := $(ALPHA_MB_LOGOS) $(FLATTEN_MB_LOGOS) $(GREENLESS_MB_LOGOS)

TARGETS := $(PDF_TARGETS) $(ALPHA_PNG_TARGETS) $(FLATTEN_PNG_TARGETS) $(ALL_MB_LOGOS)

borderwidth = 4

%.svg:
	find ../final-logos/ -type f -name $@ -exec ln -s '{}' $@ ';' -quit

%.pdf: %.svg
	inkscape -D -z --file=$< --export-pdf=$@ --export-latex

%-alpha.png: %.pdf
	convert $< -bordercolor none -border $(borderwidth) $@

%-flatten.png: %.pdf
	convert $< -bordercolor none -border $(borderwidth) \( +clone -fill White -colorize 100%% -background Black -flatten -morphology Dilate Disk:$(borderwidth) -blur 0x1 -alpha Copy -fill White -colorize 100%% \) +swap -composite -background '#5AA854' -flatten $@

%-greenless.png: %.pdf
	convert $< -bordercolor none -border $(borderwidth) \( +clone -fill White -colorize 100%% -background Black -flatten -morphology Dilate Disk:$(borderwidth) -blur 0x1 -alpha Copy -fill White -colorize 100%% \) +swap -composite -transparent '#5AA854' $@

default: all cleaninputs cleantmp

all: $(TARGETS)

cleaninputs:
	rm -f $(PDF_INPUTS) $(PDF_TEX_INPUTS) $(SVG_INPUTS) $(INPUT_MB_LOGOS) $(PDF_INPUT_MB_LOGOS) $(PDF_TEX_INPUT_MB_LOGOS)

cleantmp:
	rm -f $(patsubst %.tex,%.aux,$(SOURCE)) $(patsubst %.tex,%.log,$(SOURCE))

clean: cleaninputs cleantmp
	rm -f $(TARGETS)

.PHONY: all cleaninputs cleantmp clean default

MourningOfficeBrainz_logo_no_text.svg: ../final-logos/OfficeBrainz/VECTOR/OfficeBrainz_logo_no_text.svg
	sed 's/ffde00/000000/g;s/e3c604/333333/g' < $< > $@

OfficeMourningBrainz_logo_no_text.svg: ../final-logos/OfficeBrainz/VECTOR/OfficeBrainz_logo_no_text.svg
	sed 's/eb743b/000000/g;s/d3562c/333333/g' < $< > $@

$(PDF_TARGETS): $(SOURCE) $(PDF_INPUTS)
	pdflatex "\def\seventeen$(patsubst $(patsubst %.tex,%,$<)-%.pdf,%,$@){}\input{$<}"
	mv $(patsubst %.tex,%.pdf,$<) $@

