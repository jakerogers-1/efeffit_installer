# This makefile for Readline library documentation is in -*- text -*- mode.
# Emacs likes it that way.

# Copyright (C) 1996 Free Software Foundation, Inc.

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

topdir = ..
srcdir = .
VPATH = .

prefix = /usr/local
infodir = ${prefix}/info

mandir = ${prefix}/man
manpfx = man

man1ext = .1
man1dir = $(mandir)/$(manpfx)1
man3ext = .3
man3dir = $(mandir)/$(manpfx)3

# set this to a value to have the HTML documentation installed
htmldir =

# Support an alternate destination root directory for package building
DESTDIR =

SHELL = /bin/sh
RM = rm -f

INSTALL = /usr/bin/install -c
INSTALL_DATA = ${INSTALL} -m 644

BUILD_DIR = /usr/lib/ifeffit-1.2.5_lucas/readline
TEXINPUTDIR = $(srcdir)

MAKEINFO    = LANGUAGE= makeinfo
TEXI2DVI    = $(srcdir)/texi2dvi
TEXI2HTML   = $(srcdir)/texi2html
QUIETPS     = #set this to -q to shut up dvips
PAPERSIZE   = letter
PSDPI       = 300	# I don't have any 600-dpi printers
DVIPS       = dvips -D ${PSDPI} $(QUIETPS) -t ${PAPERSIZE} -o $@     # tricky

RLSRC = $(srcdir)/rlman.texinfo $(srcdir)/rluser.texinfo \
	$(srcdir)/rltech.texinfo $(srcdir)/manvers.texinfo \
	$(srcdir)/rluserman.texinfo
HISTSRC = $(srcdir)/hist.texinfo $(srcdir)/hsuser.texinfo \
	  $(srcdir)/hstech.texinfo $(srcdir)/manvers.texinfo

# This should be a program that converts troff to an ascii-readable format
NROFF       = groff -Tascii

# This should be a program that converts troff to postscript
GROFF       = groff

DVIOBJ = readline.dvi history.dvi rluserman.dvi
INFOOBJ = readline.info history.info rluserman.info
PSOBJ = readline.ps history.ps rluserman.ps readline_3.ps history_3.ps
HTMLOBJ = readline.html history.html rluserman.html
TEXTOBJ = readline.0 history.0

INTERMEDIATE_OBJ = rlman.dvi hist.dvi rluserman.dvi

DIST_DOCS = $(DVIOBJ) $(PSOBJ) $(HTMLOBJ) $(INFOOBJ) $(TEXTOBJ)

.SUFFIXES:      .0 .3 .ps .txt .dvi

.3.0:
	$(RM) $@
	-${NROFF} -man $< > $@

all: info dvi html ps text
nodvi: info html text

readline.dvi: $(RLSRC)
	TEXINPUTS=.:$(TEXINPUTDIR):$$TEXINPUTS $(TEXI2DVI) $(srcdir)/rlman.texinfo
	mv rlman.dvi readline.dvi

readline.info: $(RLSRC)
	$(MAKEINFO) --no-split -I $(TEXINPUTDIR) -o $@ $(srcdir)/rlman.texinfo

rluserman.dvi: $(RLSRC)
	TEXINPUTS=.:$(TEXINPUTDIR):$$TEXINPUTS $(TEXI2DVI) $(srcdir)/rluserman.texinfo

rluserman.info: $(RLSRC)
	$(MAKEINFO) --no-split -I $(TEXINPUTDIR) -o $@ $(srcdir)/rluserman.texinfo

history.dvi: ${HISTSRC}
	TEXINPUTS=.:$(TEXINPUTDIR):$$TEXINPUTS $(TEXI2DVI) $(srcdir)/hist.texinfo
	mv hist.dvi history.dvi

history.info: ${HISTSRC}
	$(MAKEINFO) --no-split -I $(TEXINPUTDIR) -o $@ $(srcdir)/hist.texinfo

readline.ps:	readline.dvi
	$(RM) $@
	$(DVIPS) readline.dvi

rluserman.ps:	rluserman.dvi
	$(RM) $@
	$(DVIPS) rluserman.dvi

history.ps:	history.dvi
	$(RM) $@
	$(DVIPS) history.dvi

#
# This leaves readline.html and rlman.html -- rlman.html is for www.gnu.org
#
readline.html:	${RLSRC}
	$(TEXI2HTML) -menu -monolithic -I $(TEXINPUTDIR) $(srcdir)/rlman.texinfo
	sed -e 's:rlman.html:readline.html:g' rlman.html > readline.html

rluserman.html:	${RLSRC}
	$(TEXI2HTML) -menu -monolithic -I $(TEXINPUTDIR) $(srcdir)/rluserman.texinfo

history.html:	${HISTSRC}
	$(TEXI2HTML) -menu -monolithic -I $(TEXINPUTDIR) $(srcdir)/hist.texinfo
	sed -e 's:hist.html:history.html:g' hist.html > history.html
	$(RM) hist.html

info:	$(INFOOBJ)
dvi:	$(DVIOBJ)
ps:	$(PSOBJ)
html:	$(HTMLOBJ)
text:	$(TEXTOBJ)

readline.0: readline.3

readline_3.ps: readline.3
	${RM} $@
	${GROFF} -man < $(srcdir)/readline.3 > $@ 

history.0: history.3

history_3.ps: history.3
	${RM} $@
	${GROFF} -man < $(srcdir)/history.3 > $@ 

clean:
	$(RM) *.aux *.cp *.fn *.ky *.log *.pg *.toc *.tp *.vr *.cps *.pgs \
	      *.fns *.kys *.tps *.vrs *.bt *.bts *.o core *.core

mostlyclean: clean

distclean: clean maybe-clean
	$(RM) $(INTERMEDIATE_OBJ)
	$(RM) Makefile

maybe-clean:
	-if test "X$(topdir)" != "X$(BUILD_DIR)"; then \
		$(RM) $(DIST_DOCS); \
	fi

maintainer-clean: clean
	$(RM) $(DIST_DOCS)
	$(RM) $(INTERMEDIATE_OBJ)
	$(RM) Makefile

installdirs:	$(topdir)/support/mkdirs
	-$(SHELL) $(topdir)/support/mkdirs $(DESTDIR)$(infodir) $(DESTDIR)$(man3dir)
	-if test -n "${htmldir}" ; then \
		$(SHELL) $(topdir)/support/mkdirs $(DESTDIR)$(htmldir) ; \
	fi

install:	installdirs
	if test -f readline.info; then \
		${INSTALL_DATA} readline.info $(DESTDIR)$(infodir)/readline.info; \
	else \
		${INSTALL_DATA} $(srcdir)/readline.info $(DESTDIR)$(infodir)/readline.info; \
	fi
	if test -f rluserman.info; then \
		${INSTALL_DATA} rluserman.info $(DESTDIR)$(infodir)/rluserman.info; \
	else \
		${INSTALL_DATA} $(srcdir)/rluserman.info $(DESTDIR)$(infodir)/rluserman.info; \
	fi
	if test -f history.info; then \
		${INSTALL_DATA} history.info $(DESTDIR)$(infodir)/history.info; \
	else \
		${INSTALL_DATA} $(srcdir)/history.info $(DESTDIR)$(infodir)/history.info; \
	fi
	-if $(SHELL) -c 'install-info --version' >/dev/null 2>&1; then \
		install-info --dir-file=$(DESTDIR)$(infodir)/dir \
			$(DESTDIR)$(infodir)/readline.info ; \
		install-info --dir-file=$(DESTDIR)$(infodir)/dir \
			$(DESTDIR)$(infodir)/history.info ; \
		install-info --dir-file=$(DESTDIR)$(infodir)/dir \
			$(DESTDIR)$(infodir)/rluserman.info ; \
	else true; fi
	-${INSTALL_DATA} $(srcdir)/readline.3 $(DESTDIR)$(man3dir)/readline$(man3ext)
	-${INSTALL_DATA} $(srcdir)/history.3 $(DESTDIR)$(man3dir)/history$(man3ext)
	-if test -n "${htmldir}" ; then \
		if test -f readline.html; then \
			${INSTALL_DATA} readline.html $(DESTDIR)$(htmldir)/readline.html; \
		else \
			${INSTALL_DATA} $(srcdir)/readline.html $(DESTDIR)$(htmldir)/readline.html; \
		fi ; \
		if test -f history.html; then \
			${INSTALL_DATA} history.html $(DESTDIR)$(htmldir)/history.html; \
		else \
			${INSTALL_DATA} $(srcdir)/history.html $(DESTDIR)$(htmldir)/history.html; \
		fi ; \
		if test -f rluserman.html; then \
			${INSTALL_DATA} rluserman.html $(DESTDIR)$(htmldir)/rluserman.html; \
		else \
			${INSTALL_DATA} $(srcdir)/rluserman.html $(DESTDIR)$(htmldir)/rluserman.html; \
		fi ; \
	fi

uninstall:
	$(RM) $(DESTDIR)$(infodir)/readline.info
	$(RM) $(DESTDIR)$(infodir)/rluserman.info
	$(RM) $(DESTDIR)$(infodir)/history.info
	$(RM) $(DESTDIR)$(man3dir)/readline$(man3ext)
	$(RM) $(DESTDIR)$(man3dir)/history$(man3ext)
	-if test -n "${htmldir}" ; then \
		$(RM) $(DESTDIR)$(htmldir)/readline.html ; \
		$(RM) $(DESTDIR)$(htmldir)/rluserman.html ; \
		$(RM) $(DESTDIR)$(htmldir)/history.html ; \
	fi
