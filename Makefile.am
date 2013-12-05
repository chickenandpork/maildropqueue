DIST_SUBDIRS =	java-pre maildropqueue tests-autotest
SUBDIRS =	java-pre maildropqueue

if DO_AUTOTEST
SUBDIRS +=	tests-autotest
endif

# if missing mkinstalldirs install-sh trigger errors, ignore the result code on the "system (cp ...)" command in automake
CONFIG_CLEAN_FILES = aclocal.m4 config.guess config.sub config.h config.h.in ltconfig ltmain.sh missing mkinstalldirs install-sh Doxyfile @PACKAGE@.spec @PACKAGE@.jar
EXTRA_DIST = @PACKAGE@.spec gitdescribe

ACLOCAL_AMFLAGS = -I m4

if DO_DOXYGEN
doc: Doxyfile
	$(DOXYGEN)

if DO_ASTYLE
formatting: maildropqueue/MailDropQueue.java.in
	$(MAKE) check
	for i in $^; do $(ASTYLE) -A1 $$i; done
	$(MAKE) check
endif
endif

if DO_GITFORMATTING
gitdescribe: .git/ORIG_HEAD
	git describe > $@
endif DO_GITFORMATTING

