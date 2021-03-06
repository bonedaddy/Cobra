# Cobra - interactive static structural code analyzer
# Sample background checkers, linked to the Cobra
# front-end in c.ar, using the api defined in c_api.h
# See the license file in ../src

CWE=cwe.o cwe_util.o \
	cwe_119.o cwe_120.o cwe_131.o \
	cwe_134.o cwe_170.o cwe_197.o \
	cwe_468.o cwe_805.o cwe_416.o \
	cwe_457.o

OTHER=abstract \
	scope_check \
	binop \
	cfg \
	deref \
	fct_param_counts \
	flatten \
	float \
	ident_check \
	ident_length \
	ifelseif \
	igrep \
	lf \
	misra2004 \
	nomacros \
	nr_cases \
	rule23_rule31 \
	switch_default

.PHONY: all install clean
LDFLAGS?=c.ar -pthread
CC?=gcc
F?=-O2
		# on mac: make LDFLAGS=c.ar
		# gprof: make F="-g -pg"
CFLAGS=-Wall -pedantic -Werror -Wshadow ${F} -DYY_NO_INPUT -I. -std=c99

all:	c.ar cwe scope_check find_taint

cwe:	cwe.h c.ar c_api.h $(CWE)
	$(CC) $(CFLAGS) -o cwe $(CWE) $(LDFLAGS)

scope_check: c.ar c_api.h scope_check.c
	$(CC) $(CFLAGS) -o scope_check scope_check.c $(LDFLAGS)

c.ar:
	cd ../src; make c.ar

find_taint:	find_taint.c cwe_util.c
	$(CC) $(CFLAGS) -o find_taint find_taint.c cwe_util.c $(LDFLAGS)

$(OTHER): c_api.h c.ar

install:
	@echo "use one of: install_linux, install_cygwin, or install_mac"

install_linux: all
	cp cwe scope_check find_taint ../bin_linux

install_cygwin: all
	cp *.exe ../bin_cygwin

install_mac: all
	cp cwe scope_check find_taint ../bin_mac

clean:
	rm -f *.exe *.o cwe $(OTHER)
