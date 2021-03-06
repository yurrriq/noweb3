# Copyright 1991-1998 by Norman Ramsey.  All rights reserved.
# See file COPYRIGHT for more information.
#
# don't edit this file; edit ../nwconfig instead

include ../nwconfig

################################################################
# CII is directory where you installed CII ($BUILDDIR)

CII=../cii
LUA=../lua-2.5+nw

CFLAGS=-g -I$(CII)/include -I$(LUA)/include -L$(CII) -L$(LUA)/lib \
	-DPLATFORM='"$(PLATFORM)"' -Dnw_$(PLATFORM) \
	-DNWDEFAULTDIR='"$(LIB3)"' $(XCFLAGS)

# after installation, make doc.dvi for literate version

# list of .o files for each target

FINDUSESOBJS=columns.o errors.o finduses.o match.o getline.o recognize.o fu-run.o \
             nwbuffer.o fromascii.o ipipe.o
MNTOBJS=mnt.o getline.o match.o modules.o modtrees.o notangle.o \
	strsave.o errors.o columns.o
TANGLEOBJS=notangle.o getline.o match.o modules.o modtrees.o strsave.o \
	main.o errors.o columns.o nwbuffer.o nt-run.o fromascii.o ipipe.o cpif.o
MARKUPOBJS=markparse.o strsave.o markup.o errors.o getline.o columns.o \
	ipipe.o nwbuffer.o markup-run.o
NOOBJS=xpipe.o xpipe-lua.o nwprocess.o nwtime.o lua-help.o nwbuffer.o ipipe.o \
	ipipe-lua.o env-lua.o markparse.o strsave.o markup.o errors.o \
	getline.o columns.o stages.o lua-main.o cpif.o \
	mpipe.o mpipe-lua.o fromascii.o notangle.o main.o modules.o modtrees.o \
	util.o sys.o noweb-lua.o nwbuffer-lua.o misc-lua.o finduses.o recognize.o 
NWMTIMEOBJS=nwmtime.o getline.o errors.o columns.o
EXTERNAL=finduses markup nt mnt nwmtime

FILES=markparse.nw markup.nw \
      main.nw notangle.nw match.nw mnt.nw modules.nw modtrees.nw \
      finduses.nw recognize.nw cpif.nw \
      getline.nw columns.nw errors.nw strsave.nw \
      nwbuffer.nw nwbuffer-lua.nw \
      ipipe.nw ipipe-lua.nw xpipe.nw xpipe-lua.nw mpipe.nw mpipe-lua.nw \
      nwprocess.nw nwtime.nw lua-main.nw nwmtime.nw

# sources to build when distributing C code
#   (add all generated .c and .h files to this list)

SRCS=columns.h errors.h getline.h markup.h match.h modtrees.h \
	modules.h notangle.h recognize.h strsave.h cpif.h cpif.c \
	columns.c errors.c getline.c finduses.c main.c markparse.c markup.c match.c \
	mnt.c modtrees.c modules.c notangle.c readme.c recognize.c strsave.c \
  cargs.h env-lua.c env-lua.h fromascii.c fromascii.h ipipe-lua.c \
  ipipe-lua.h ipipe.c ipipe.h lua-help.c lua-help.h lua-main.c lua-main.h \
  markparse.h markup-run.c mpipe-lua.c mpipe-lua.h mpipe.c mpipe.h \
  noweb-lua.c noweb-lua.h nt-run.c nwbuffer.c \
  nwbuffer.h util.c util.h precompiled.h stages.c stages.h sys.c sys.h \
  toascii.c toascii.h xpipe-lua.c xpipe-lua.h xpipe.c xpipe.h \
  nwbuffer-lua.c nwprocess.h nwprocess.c nwtime.h nwtime.c \
     fu-run.c misc-lua.c misc-lua.h   markup.ps \
     nwmtime.c
NAME='name of RCS version'

NOTANGLE=notangle
TANGLEOPTS=-filter "nocond $(PIPE)"
SHELL=/bin/sh

# set this for CPIF and then distribute tools with bad timestamps...
#CPIF=| cpif
CPIF=>

# metarules so you don't need a new rule for each .c and .h

.SUFFIXES: .nw .tex .dvi .h .html .dvi .ps
.nw.tex: ;	no weave -ifilter "Stages.elide, 'hackers:*'" $*.nw >$*.tex
.nw.c: ;	$(NOTANGLE) $(TANGLEOPTS) -L $*.nw >$*.c
.nw.o: ;	$(NOTANGLE) $(TANGLEOPTS) -L $*.nw >$*.c
		$(CC) $(CFLAGS) -c $*.c
.nw.h: ;	$(NOTANGLE) $(TANGLEOPTS) -L -Rheader $*.nw $(CPIF) $*.h
.nw.html: ;	no weave -ifilter "Stages.elide, 'hackers:*'" -filter l2h -autodefs c -index -html $*.nw >$*.html
.tex.dvi: ;	latex '\scrollmode \input{$*}' ;
		latex '\scrollmode \input{$*}'
.dvi.ps: ;	dvips -o $*.ps $*.dvi


all:	no nwmtime

external: $(EXTERNAL)

nt:	$(TANGLEOBJS)
	$(CC) $(CFLAGS) -o nt $(TANGLEOBJS) -lcii 

finduses:	$(FINDUSESOBJS)
	$(CC) $(CFLAGS) -o finduses $(FINDUSESOBJS) -lcii

markup:	$(MARKUPOBJS)
	$(CC) $(CFLAGS) -o markup $(MARKUPOBJS) -lcii

mnt:	$(MNTOBJS)
	$(CC) $(CFLAGS) -o mnt $(MNTOBJS)

nwmtime:	$(NWMTIMEOBJS)
	$(CC) $(CFLAGS) -o nwmtime $(NWMTIMEOBJS)

nwbuffer-lua.c: nwbuffer.nw nwbuffer-lua.nw
	(echo "<<key macros>>="; $(NOTANGLE) $(TANGLEOPTS) -R'key macros' nwbuffer.nw; echo "@") | $(NOTANGLE) $(TANGLEOPTS) -L nwbuffer-lua.nw - > $@

# did have  $(CII)/memchk.o preceding -lcii
no:	$(NOOBJS) $(CII)/memchk.o $(CII)/libcii.a $(LUA)/lib/liblua.a $(LUA)/lib/liblualib.a
	$(CC) $(CFLAGS) -o no $(NOOBJS) -lcii -llua -llualib -lm


source:	$(SRCS)
touch: $(SRCS)
	touch $(SRCS)

clean: ;	rm -f nt markup mnt finduses fakepretty no buildswitch nwmtime
		rm -f core *.makelog *.tex *.log *.blg *.dvi *.o *.toc *~ 
		rm -f *.atac *.trace *.html *.icn

clobber:	clean
		rm -f $(SRCS) TAGS doc.ps overview.ps fakepretty.c pretty.[ch]
		rm -f *.aux *.bbl

checkin:	$(FILES) doc.nw
		ci $(CINAME) $(CIMSG) -l $(FILES) doc.nw

doc.tex:	doc.nw
	cp doc.nw doc.tex

allcode.tex: $(FILES) Makefile
	no weave -ifilter "Stages.elide, 'hackers:*'" -n -index $(FILES) > allcode.tex

doc.dvi:	doc.tex allcode.tex
	latex doc

markup.ps: markparse.nw
	notangle -Rmarkup.dot markparse.nw | dot -Tps > markup.ps

FPOBJS=fakepretty.o pretty.o errors.o getline.o match.o strsave.o columns.o

fakepretty.o: pretty.h

fakepretty: $(FPOBJS)
	$(CC) $(CFLAGS) -o fakepretty $(FPOBJS)


################################################################

overview.tex: overview.nw
	no weave -delay overview.nw > overview.tex

depend:
	gcc $(CFLAGS) -MM -MG *.c

################################################################

columns.o: columns.c columns.h
cpif.o: cpif.c cpif.h errors.h strsave.h
errors.o: errors.c errors.h
finduses.o: errors.h match.h getline.h recognize.h nwbuffer.h
getline.o: getline.c columns.h errors.h getline.h
ipipe.o: ipipe.c ipipe.h nwbuffer.h
main.o: main.c notangle.h errors.h columns.h modules.h modtrees.h \
	nwbuffer.h cargs.h cpif.h
markparse.o: markparse.c errors.h markup.h getline.h columns.h \
 nwbuffer.h markparse.h
markup.o: markup.c markup.h strsave.h errors.h
markup-test.o: markup-test.c markparse.h nwbuffer.h  ipipe.h 
match.o: match.c match.h
modtrees.o: modtrees.c strsave.h modules.h modtrees.h errors.h
modules.o: modules.c modules.h modtrees.h errors.h columns.h strsave.h
notangle.o: notangle.c strsave.h getline.h modules.h modtrees.h \
 errors.h match.h notangle.h nwbuffer.h
nwbuffer.o: nwbuffer.h
strsave.o: strsave.c strsave.h errors.h

env-lua.o: env-lua.c env-lua.h lua-help.h
fromascii.o: getline.h nwbuffer.h errors.h
ipipe.o: ipipe.c ipipe.h nwbuffer.h nwtime.h
ipipe-lua.o: ipipe-lua.c ipipe-lua.h
lua-help.o: lua-help.c lua-help.h
lua-main.o: lua-main.c lua-main.h util.h misc-lua.h
util.o: util.c precompiled.h sys.h
sys.o: sys.c
lua-main.o: lua-main.c xpipe-lua.h ipipe-lua.h env-lua.h stages.h noweb-lua.h \
  mpipe-lua.h misc-lua.h
mpipe.o: mpipe.h ipipe.h xpipe.h nwbuffer.h fromascii.h 
mpipe-lua.o: mpipe.h lua-help.h
misc-lua.o: lua-help.h
nwprocess.o: nwprocess.h
nwtime.o: nwtime.h
stages.o: stages.c nwbuffer.h  fromascii.h \
	 lua-help.h cargs.h markparse.h notangle.h
xpipe.o: xpipe.c xpipe.h sys.h errors.h util.h nwprocess.h
xpipe-lua.o: xpipe-lua.c xpipe-lua.h xpipe.h lua-help.h cargs.h
nt-run.o: nt-run.c markparse.h nwbuffer.h ipipe.h cargs.h fromascii.h \
	nwbuffer.h errors.h

