nodist_pkgdata_JAVA = MailDropQueue.java SendMime.java
nodist_pkgdata_DATA = mdq.jar 

EXTRA_DIST = $(nodist_pkgdata_JAVA)  MailDropQueue.java.in
CONFIG_CLEAN_FILES = @PACKAGE@.jar

AM_JAVACFLAGS= -cp $(top_builddir)/$(subdir)/mail.jar:$(top_builddir)/java-pre/getopt.jar:$(top_builddir)/$(subdir)/subethasmtp-3.1.7.jar:$(top_builddir)/$(subdir)/activation.jar:$(top_builddir)/$(subdir)/jsr305-1.3.9.jar:$(top_builddir)

BUILDTMP=$(shell date +%s)
BUILT_SOURCES =


BUILT_SOURCES += jsr305-1.3.9.jar

jsr305-1.3.9.jar: jsr305-1.3.9.jar.zip
	unzip -o -j $< $@ && touch $@

jsr305-1.3.9.jar.zip:
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://www.java2s.com/Code/JarDownload/jsr305/jsr305-1.3.9.jar.zip || curl -o $@ http://www.java2s.com/Code/JarDownload/jsr305/jsr305-1.3.9.jar.zip
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)
	touch $@


BUILT_SOURCES += subethasmtp-3.1.7.jar

subethasmtp-3.1.7.zip:
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://subethasmtp.googlecode.com/files/$(@F) || curl -o $@ http://subethasmtp.googlecode.com/files/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)
	touch $@

subethasmtp-3.1.7.jar: subethasmtp-3.1.7.zip
	BASE=$$(echo $$(basename $< .zip)); unzip -o -j $< $${BASE}/$@ && touch $@


BUILT_SOURCES += activation.jar

activation.jar: subethasmtp-3.1.7.zip
	BASE=$$(echo $$(basename $< .zip)); unzip -o -j $< $${BASE}/lib/$@ && touch $@


#dnl BUILT_SOURCES += junit-4.5.jar

#dnl junit-4.5.jar 

BUILT_SOURCES += mail.jar

mail.jar: javamail1_4_5.zip
	unzip -o -j $< javamail-1.4.5/$@ && touch $@

javamail1_4_5.zip:
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://vipars-repository.googlecode.com/svn-history/r15/MailClient/$(@F) || curl -Lo $@ http://vipars-repository.googlecode.com/svn-history/r15/MailClient/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)
	touch $@


BUILT_SOURCES += slf4j-api-1.5.6.jar slf4j-simple-1.5.6.jar

slf4j-api-1.5.6.jar slf4j-simple-1.5.6.jar: slf4j-1.5.6.zip
	BASE=$$(echo $$(basename $< .zip)); unzip -o -j $< $${BASE}/$@ && touch $@

slf4j-1.5.6.zip: 
	test -f $${TMPDIR-/tmp}/$(@F) && cp $${TMPDIR-/tmp}/$(@F) $@ || true
	test -f $@ || wget -O $@ http://www.slf4j.org/dist/$(@F) || curl -o $@ http://www.slf4j.org/dist/$(@F)
	test -f $${TMPDIR-/tmp}/$(@F) || cp $@ $${TMPDIR-/tmp}/$(@F)
	touch $@



mdq.jar: classnodist_pkgdata.stamp
	awk '/^package / {gsub (";$$",""); gsub ("\\.","/",$$2); SUBS[$$2]=$$2 } END { FIRST=0; for (n in SUBS) { if (0 == FIRST++) printf "jar cvf $@"; printf " -C .. %s",n }; printf "\n"; }' $(nodist_pkgdata_JAVA) | $(SHELL)
	awk -v BUILDTOP="$(top_builddir)" '/^package / {gsub (";$$",""); gsub ("\\.","/",$$2); SUBS[$$2]=$$2 } END { FIRST=0; for (n in SUBS) { if (0 == FIRST++) printf "jar cvef org.smallfoot.mdq.MailDropQueue $@"; printf " -C %s %s",BUILDTOP,n }; printf "\n"; }' $(nodist_pkgdata_JAVA) | $(SHELL)
	touch $(subdir).class

clean-local:
	CLASSES=`for i in $(nodist_pkgdata_JAVA); do awk -v FILE="$$i" '/^package / {gsub (";$$",""); gsub (".java",".class",FILE); gsub ("\\\\.","/",$$2); printf "%s/%s\n",$$2,FILE}' $(srcdir)/$$i; done`; (cd $(top_builddir)/ && rm -f $$CLASSES)


@PACKAGE@.jar: $(BUILT_SOURCES) mdq.jar $(top_builddir)/java-pre/getopt.jar
	@-rm -fr $(@D)/.workdir
	@mkdir -p $(@D)/.workdir
	@echo "Manifest-Version: 1.0" > $(@D)/.workdir/MANIFEST.MF
	@echo "Created-By: @PACKAGE@-@VERSION@" >> $(@D)/.workdir/MANIFEST.MF
	echo "Main-Class: org.smallfoot.mdq.MailDropQueue"  >> $(@D)/.workdir/MANIFEST.MF
	ln -f $^ $(@D)/.workdir/
	for i in $(^F) ; do (cd $(@D)/.workdir  && jar xf $$i) ; done
	rm -fr $(@D)/.workdir/META-INF/{LICENSE.txt,MANIFEST.MF,{SUN_MICR,TESTKEY}.{RSA,SF}}
	(cd $(@D)/.workdir && jar cm MANIFEST.MF META-INF com gnu javax org) > $@
	rm -fr $(@D)/.workdir
	touch $@

#curl -O http://subethasmtp.googlecode.com/files/subethasmtp-3.1.7.zip
