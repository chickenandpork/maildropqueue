nodist_pkgdata_DATA = getopt.jar

EXTRA_DIST = 
CONFIG_CLEAN_FILES = java-getopt-1.0.13.jar getopt.jar

AM_JAVACFLAGS= -cp $(top_builddir)/$(subdir)/java-getopt-1.0.13.jar:$(top_builddir)
BUILDTMP=$(shell date +%s)

BUILT_SOURCES = java-getopt-1.0.13.jar

java-getopt-1.0.13.jar: 
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://download.java.net/maven/2/gnu/getopt/java-getopt/1.0.13/$(@F) || curl -o $@ http://download.java.net/maven/2/gnu/getopt/java-getopt/1.0.13/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)

clean-local:
	CLASSES=`for i in $(nodist_pkgdata_JAVA); do awk -v FILE="$$i" '/^package / {gsub (";$$",""); gsub (".java",".class",FILE); gsub ("\\\\.","/",$$2); printf "%s/%s\n",$$2,FILE}' $(srcdir)/$$i; done`; (cd $(top_builddir)/ && rm -f $$CLASSES)


getopt.jar: java-getopt-1.0.13.jar
	@-rm -fr $(@D)/.workdir
	@mkdir -p $(@D)/.workdir
	@echo "Manifest-Version: 1.0" > $(@D)/.workdir/MANIFEST.MF
	@echo "Created-By: @PACKAGE@-@VERSION@" >> $(@D)/.workdir/MANIFEST.MF
	echo "Main-Class: org.smallfoot.getopt.GetOpt"  >> $(@D)/.workdir/MANIFEST.MF
	ln -f $^ $(@D)/.workdir/
	for i in $(^F) ; do (cd $(@D)/.workdir  && jar xf $$i) ; done
	find $(@D)/.workdir '(' -name \*.java -o -name \*.html -o -name Makefile -o -name ChangeLog -o -name LANGUAGES -o -name buildx.xml ')' -exec rm {} \;
	(cd $(@D)/.workdir && jar cm MANIFEST.MF gnu org) > $@
	rm -fr $(@D)/.workdir
	touch $@
