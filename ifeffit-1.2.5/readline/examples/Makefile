#
# This is the Makefile for the readline examples subdirectory.
#
# Copyright (C) 1994 Free Software Foundation, Inc.

# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA 02111 USA.
RL_LIBRARY_VERSION = 4.3

SHELL = /bin/sh
RM = rm -f

srcdir = .
VPATH = .
top_srcdir = ..
BUILD_DIR = .

# Support an alternate destination root directory for package building
DESTDIR =

DEFS = -DHAVE_CONFIG_H
CC = gcc
CFLAGS = -g -O
LOCAL_CFLAGS =  -DREADLINE_LIBRARY -DRL_LIBRARY_VERSION='"$(RL_LIBRARY_VERSION)"'
CPPFLAGS = 

INCLUDES = -I$(srcdir) -I$(top_srcdir) -I..

CCFLAGS  = $(DEFS) $(LOCAL_CFLAGS) $(CPPFLAGS) $(INCLUDES) $(CFLAGS)
LDFLAGS = -g -L..  -L./lib/termcap

READLINE_LIB = ../libreadline.a
HISTORY_LIB = ../libhistory.a

TERMCAP_LIB = -ltermcap

.c.o:
	${RM} $@
	$(CC) $(CCFLAGS) -c $<

EXECUTABLES = fileman rltest rl rlcat rlversion histexamp 
OBJECTS = fileman.o rltest.o rl.o rlversion.o histexamp.o

all: $(EXECUTABLES)
everything: all rlfe

rl: rl.o $(READLINE_LIB)
	$(CC) $(LDFLAGS) -o $@ rl.o -lreadline $(TERMCAP_LIB)

rlcat: rlcat.o $(READLINE_LIB)
	$(CC) $(LDFLAGS) -o $@ rlcat.o -lreadline $(TERMCAP_LIB)

fileman: fileman.o $(READLINE_LIB)
	$(CC) $(LDFLAGS) -o $@ fileman.o -lreadline $(TERMCAP_LIB)

rltest: rltest.o $(READLINE_LIB)
	$(CC) $(LDFLAGS) -o $@ rltest.o -lreadline $(TERMCAP_LIB)

rlversion: rlversion.o $(READLINE_LIB)
	$(CC) $(LDFLAGS) -o $@ rlversion.o -lreadline $(TERMCAP_LIB)

histexamp: histexamp.o $(HISTORY_LIB)
	$(CC) $(LDFLAGS) -o $@ histexamp.o -lhistory $(TERMCAP_LIB)

clean mostlyclean:
	$(RM) $(OBJECTS)
	$(RM) $(EXECUTABLES) *.exe
	$(RM) rlfe.o rlfe

distclean maintainer-clean: clean
	$(RM) Makefile

fileman.o: fileman.c
rltest.o: rltest.c
rl.o: rl.c
rlversion.o: rlversion.c
histexamp.o: histexamp.c

fileman.o: $(top_srcdir)/readline.h
rltest.o: $(top_srcdir)/readline.h
rl.o: $(top_srcdir)/readline.h
rlversion.o: $(top_srcdir)/readline.h
histexamp.o: $(top_srcdir)/history.h

# Stuff for Per Bothner's `rlfe' program
 
rlfe: rlfe.o $(READLINE_LIB) $(HISTORY_LIB)
	$(CC) $(LDFLAGS) -o $@ rlfe.o -lreadline -lhistory ${TERMCAP_LIB}

rlfe.o: rlfe.c

rlfe.o: $(top_srcdir)/readline.h
rlfe.o: $(top_srcdir)/history.h
