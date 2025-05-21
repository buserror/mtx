# Makefile for mtx suite

TARGET_ARCH := $(shell $(CC) -dumpmachine)
O ?= build-$(TARGET_ARCH)

VERSION = 2.0.0
PREFIX ?= /usr/local
SBINDIR = $(PREFIX)/sbin
MANDIR = $(PREFIX)/man/man1

CC = gcc
CFLAGS = -Wall -DVERSION="\"${VERSION}\"" -DLINUX
# Add -D_GNU_SOURCE if needed for strcasestr, etc.
# CFLAGS += -g # For debugging

LDFLAGS =
LIBS =

BINS = $(O)/mtx $(O)/tapeinfo $(O)/loaderinfo $(O)/scsitape $(O)/scsieject $(O)/nsmhack
DEBUG_BINS = $(O)/mam2debug $(O)/mam2debug2
ALL_BINS = $(BINS) $(DEBUG_BINS)

# Common object file
MTXL_OBJS = $(O)/mtxl.o

all: $(ALL_BINS)

# Dependencies for main executables
$(O)/mtx: $(O)/mtx.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/tapeinfo: $(O)/tapeinfo.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/loaderinfo: $(O)/loaderinfo.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/scsitape: $(O)/scsitape.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/scsieject: $(O)/scsieject.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/nsmhack: $(O)/nsmhack.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

# Dependencies for debug utilities
$(O)/mam2debug: $(O)/mam2debug.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

$(O)/mam2debug2: $(O)/mam2debug2.o $(MTXL_OBJS)
	@mkdir -p $(O)
	$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)

# Object file compilation rules
# .cpp files are treated as C, so use $(CC)
$(O)/%.o: %.cpp
	@mkdir -p $(O)
	$(CC) $(CFLAGS) -c -o $@ $<

# Specific rule for mtxl.o as it includes other .cpp files effectively
$(O)/mtxl.o: mtxl.cpp scsi_linux.cpp sg_err.cpp mtx.h mtxl.h sg_err.h
	@mkdir -p $(O)
	$(CC) $(CFLAGS) -c -o $@ mtxl.cpp

# Clean target
clean:
	rm -rf $(O)

# Install target
install: all
	mkdir -p $(DESTDIR)$(SBINDIR)
	for bin in $(BINS); do \
		install -m 755 $$bin $(DESTDIR)$(SBINDIR); \
	done
	mkdir -p $(DESTDIR)$(MANDIR)
	for manpage in mtx.1 tapeinfo.1 loaderinfo.1 scsitape.1 scsieject.1; do \
		install -m 644 $$manpage $(DESTDIR)$(MANDIR); \
	done

# Phony targets
.PHONY: all clean install
