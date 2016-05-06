.PHONY: checkout

all: commits.html

OPTS = --variable theme=solarized

clean:
	rm target/*.html

checkout:
	git clone git@github.com:hakimel/reveal.js.git
	cd reveal.js/ && git apply ../revealjs.patch && cd

%.html: %.md
	mkdir -p ./target
	pandoc -t revealjs --self-contained $^ -o target/$@ $(OPTS)
