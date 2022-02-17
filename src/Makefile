
#   Unix make file for random test program

VERSION = 1.4

CC = gcc
#CFLAGS = -g -Wall
CFLAGS = -O3 -Wall -DVERSION=\"$(VERSION)\"

ent:	ent.o iso8859.o randtest.o chisq.o
	$(CC) $(CFLAGS) ent.o iso8859.o randtest.o chisq.o -o ent -lm

ent.c:	iso8859.h randtest.h

DISTFILES = Makefile ent.c ent.html entest.mas \
		iso8859.c iso8859.h randtest.c randtest.h \
		chisq.c figures

DISTNAME = ent-$(VERSION)

dist:
	rm -rf $(DISTNAME)
	mkdir $(DISTNAME)
	cp -pr $(DISTFILES) $(DISTNAME)
	tar cfvz $(DISTNAME).tar.gz $(DISTNAME)
	rm -rf $(DISTNAME)

zip:
	rm -f random.zip
	zip -pr random.zip $(DISTFILES)

TESTFILE = figures/entitle.png

check:	ent
	./ent $(TESTFILE) >entest.bak
	./ent -c $(TESTFILE) >>entest.bak
	./ent -fc $(TESTFILE) >>entest.bak
	./ent -b $(TESTFILE) >>entest.bak
	./ent -bc $(TESTFILE) >>entest.bak
	./ent -t $(TESTFILE) >>entest.bak
	./ent -ct $(TESTFILE) >>entest.bak
	./ent -ft $(TESTFILE) >>entest.bak
	./ent -bt $(TESTFILE) >>entest.bak
	./ent -bct $(TESTFILE) >>entest.bak
	@diff entest.mas entest.bak ; if test $$? -ne 0  ; then \
		echo '** ent:  Regression test failed. **' ; else \
		echo "All tests passed." ; fi
	@rm entest.bak

lint:
	lint *.c

clean:
	rm -f *.o  ent $(DISTNAME) $(DISTNAME).tar.gz random.zip

