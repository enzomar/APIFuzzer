SHELL:=$(shell which bash)
parent_dir:=$(shell dirname $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST)))))
.PHONY: all
all: clean
all: apidoc
all: sphinx

SPHINXOPTS= -j 4 -d _build/doctrees .

.PHONY: sphinx
sphinx:
	sphinx-build -b html $(SPHINXOPTS) _build/html

.PHONY: clean
clean:
	rm -rf _build

.PHONY: apidoc
apidoc:
	sphinx-apidoc  ../apifuzzer -f -e -o .

clean-build-folder:
	rm -fr  ../dist/ ../build/ ../*.egg-info

build-pip-package: clean-build-folder
	DIR=/tmp/test
	rm -rf $DIR
	mkdir -p $DIR
	mv ../test $DIR/  # not the best, but setup exclude doesn't work
	find ../ -name "__pycache __" -exec rm -rf {} \;
	cd .. && python3 setup.py sdist bdist_wheel
	tar -tvf ../dist/APIFuzzer-*.tar.gz
	mv $DIR/test ../

upload-test-pip-package:
	cd .. && python3 -m twine upload --repository testpypi dist/* --verbose

upload-pip-package:
	cd .. && python3 -m twine upload --repository pypi dist/* --verbose
