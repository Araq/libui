# 16 october 2015

# TODO http://stackoverflow.com/questions/4122831/disable-make-builtin-rules-and-variables-from-inside-the-make-file

# silence entering/leaving messages
MAKEFLAGS += --no-print-directory

OUTDIR = out
OBJDIR = .obj

# default install prefix is /usr
ifndef prefix
	prefix=/usr
endif

# MAME does this so :/
ifeq ($(OS),Windows_NT)
	OS = windows
endif

ifndef OS
	UNAME = $(shell uname -s)
	ifeq ($(UNAME),Darwin)
		OS = darwin
	else ifeq ($(UNAME),Haiku)
		OS = haiku
	else
		OS = unix
	endif
endif

# default is to build with debug symbols
ifndef NODEBUG
	NODEBUG = 0
endif

# parameters
export OS
# TODO CC, CXX, RC, CVTRES, LD
export CFLAGS
export CXXFLAGS
# TODO RCFLAGS, CVTRESFLAGS
export LDFLAGS
export NODEBUG
export EXAMPLE

# other important variables
export OBJDIR
export OUTDIR

libui:
	@$(MAKE) -f build/GNUmakefile.libui inlibuibuild=1

clean:
	rm -rf $(OBJDIR) $(OUTDIR)

test: libui
	@$(MAKE) -f build/GNUmakefile.test inlibuibuild=1

# TODO provide a build option for the queuemaintest

example: libui
	@$(MAKE) -f build/GNUmakefile.example inlibuibuild=1

# TODO examples rule? --> That's it right ?
examples:
	for d in $$(ls examples); do $(MAKE) -f GNUmakefile example EXAMPLE=$$d ; done

.PHONY: examples

install: libui
	cp out/libui.so $(DESTDIR)$(prefix)/lib/libui.so.0
	ln -s libui.so.0 $(DESTDIR)$(prefix)/lib/libui.so
	cp ui.h ui_unix.h $(DESTDIR)$(prefix)/include/ui.h

