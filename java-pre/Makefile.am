nodist_pkgdata_DATA = getopt.jar

EXTRA_DIST = 
CONFIG_CLEAN_FILES = java-getopt-1.0.13.jar getopt.jar

AM_JAVACFLAGS= -cp $(top_builddir)/$(subdir)/java-getopt-1.0.13.jar:$(top_builddir)
BUILDTMP=$(shell date +%s)

BUILT_SOURCES = java-getopt-1.0.13.jar
BUILT_SUBDIRS = gnu

java-getopt-1.0.13.jar: 
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://download.java.net/maven/2/gnu/getopt/java-getopt/1.0.13/$(@F) || curl -o $@ http://download.java.net/maven/2/gnu/getopt/java-getopt/1.0.13/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)

if DO_SMALLFOOTGETOPT
BUILT_SOURCES += smallfoot-getopt.jar
BUILT_SUBDIRS += org

smallfoot-getopt.jar: 
	test \! -f $${TMPDIR-/tmp}/$(@F) || jar tf $${TMPDIR-/tmp}/$(@F) >/dev/null 2>/dev/null || rm $${TMPDIR-/tmp}/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ @SMALLFOOT_GETOPT_URL@ || curl -o $@ @SMALLFOOT_GETOPT_URL@ 
	test \! -f $@ || jar tf $@ >/dev/null 2>/dev/null || rm $@
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)
endif

clean-local:
	CLASSES=`for i in $(nodist_pkgdata_JAVA); do awk -v FILE="$$i" '/^package / {gsub (";$$",""); gsub (".java",".class",FILE); gsub ("\\\\.","/",$$2); printf "%s/%s\n",$$2,FILE}' $(srcdir)/$$i; done`; (cd $(top_builddir)/ && rm -f $$CLASSES)


getopt.jar: $(BUILT_SOURCES)
	@-rm -fr $(@D)/.workdir
	@mkdir -p $(@D)/.workdir
	@echo "Manifest-Version: 1.0" > $(@D)/.workdir/MANIFEST.MF
	@echo "Created-By: @PACKAGE@-@VERSION@" >> $(@D)/.workdir/MANIFEST.MF
	echo "Main-Class: org.smallfoot.getopt.GetOpt"  >> $(@D)/.workdir/MANIFEST.MF
	ln -f $^ $(@D)/.workdir/
	for i in $(^F) ; do (cd $(@D)/.workdir  && jar xf $$i) ; done
	find $(@D)/.workdir '(' -name \*.java -o -name \*.html -o -name Makefile -o -name ChangeLog -o -name LANGUAGES -o -name buildx.xml ')' -exec rm {} \;
	(cd $(@D)/.workdir && jar cm MANIFEST.MF $(BUILT_SUBDIRS) ) > $@
	rm -fr $(@D)/.workdir
	touch $@
