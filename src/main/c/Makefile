ROOT := $(realpath ../../..)
RUBY := $(ROOT)/bin/truffleruby

ifdef SULONG_HOME
goal: all
else
goal: no_sulong
endif

no_sulong: clean
	@echo "WARNING: SULONG_HOME not set - not building cexts" 1>&2

all: $(ROOT)/lib/cext/ruby.su $(ROOT)/lib/mri/openssl.su

clean:
	rm -f $(ROOT)/lib/cext/ruby.su
	rm -f cext/Makefile cext/*.bc cext/ruby.su
	rm -f $(ROOT)/lib/mri/openssl.su
	rm -f openssl/Makefile openssl/*.bc openssl/openssl.su

# ruby.su
cext/Makefile: cext/extconf.rb
	cd cext && $(RUBY) extconf.rb

cext/ruby.su: cext/Makefile cext/ruby.c $(ROOT)/lib/cext/ruby.h
	cd cext && $(MAKE)

$(ROOT)/lib/cext/ruby.su: cext/ruby.su
	cp $< $@

# openssl.su
openssl/Makefile: openssl/extconf.rb
	cd openssl && $(RUBY) extconf.rb

openssl/openssl.su: openssl/Makefile openssl/*.c openssl/*.h
	cd openssl && $(MAKE)

$(ROOT)/lib/mri/openssl.su: openssl/openssl.su
	cp $< $@
