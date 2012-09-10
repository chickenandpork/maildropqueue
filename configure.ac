dnl ***********************************************************************
dnl Process this file with autoconf to produce a configure script.
dnl clearly this is meant for those who have never used autoconf.
dnl
dnl the fasttrack to using this: "autoreconf -vfi && ./configure && make"
dnl ***********************************************************************
#AC_INIT(install-sh)
AC_INIT(maildropqueue, 0.2)
AC_CONFIG_MACRO_DIR([m4])

AC_CANONICAL_HOST

AM_INIT_AUTOMAKE([foreign dist-bzip2])
AC_CONFIG_TESTDIR([tests-autotest])

dnl ***********************************************************************
dnl Checks for the bare-minumum of programs to get a java build going
dnl ***********************************************************************
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_MAKE_SET

AC_SUBST(AUTOM4TE, autom4te)

AC_PATH_PROGS(XMLLINT, [xmllint], [missing xmllint])
AC_ARG_WITH(xmllintcommentindent,
[AS_HELP_STRING([--with-xmllintcommentindent],[Set XML-comment indentation fudge such as xmllint-2.6.x/2.7.x differences; default=detect])],
[
    case "$withval" in
	"yes") WITH_XMLLINTINDENT="  " ;;
	"no")  WITH_XMLLINTINDENT=""  ;;
	*) case $(xmllint --version 2>&1 | awk '/version/ {print $NF}') in
	    206*) WITH_XMLLINTINDENT="" ;;
	    *) WITH_XMLLINTINDENT="  "  ;;
	    esac ;;
    esac
],
[
    case $(xmllint --version 2>&1 | awk '/version/ {print $NF}') in
	206*) WITH_XMLLINTINDENT="" ;;
	*) WITH_XMLLINTINDENT="  "  ;;
    esac
])
AC_SUBST([XMLLINTINDENT], ${WITH_XMLLINTINDENT})

AC_ARG_WITH(doxygen,
[AS_HELP_STRING([--with-doxygen],[Indicate Doxygen allowing a without-; (default=search)])],
[
    case "$withval" in
	""|"yes") 
		AC_PATH_PROGS(DOXYGEN, [doxygen], missing, $PATH:/Applications/Doxygen.app/Contents/Resources)
		AC_PATH_PROGS(FOP, [fop], missing)
		;;
	"no")  DOXYGEN=missing ;;
	*) DOXYGEN=$withval ;;
    esac
],
[
	DOXYGEN=missing 
])
AC_PATH_PROGS(AWK, [awk gawk nawk], [missing awk])
AC_PATH_PROGS(DOT, [dot], missing, $PATH:/usr/local/bin:/usr/local/graph*/bin)
AC_SUBST(PATHDOT, [`dirname "${DOT}"`])
AM_CONDITIONAL(DO_DOT, test "x$DOT" != "xmissing")
AM_CONDITIONAL(DO_DOXYGEN, test "x$DOXYGEN" != "xmissing" -a "x$DOT" != "xmissing")
if test "x$DOT" != "xmissing"; then HAVE_DOT=yes; else HAVE_DOT=no; fi
AC_SUBST(HAVE_DOT)
AC_PATH_PROGS(ASTYLE, [astyle], missing, $PATH:$HOME/bin)
AM_CONDITIONAL(DO_ASTYLE, test "x$ASTYLE" != "xmissing")
AC_PATH_PROGS(MD5, [md5 md5sum], missing)
CHECKMD5=$(${MD5} tests-autotest/Makefile.am | ${AWK} '{if (2 == NF) print $1 ; else print $NF;}')
UCHECKMD5=$(echo "${CHECKMD5}" | tr "[a-z]" "[A-Z]")
AC_SUBST([CHECKMD5], ${CHECKMD5})
AC_SUBST([UCHECKMD5], ${UCHECKMD5})
AC_PATH_PROGS(RPMBUILD, [rpmbuild], [missing rpmbuild])
AM_CONDITIONAL(HAVE_RPMBUILD, [test "x$RPMBUILD" != "xmissing rpmbuild"])

SVN_CHECK_JDK([1.6.0])
AM_CONDITIONAL(DO_JAVA, test "$JAVAC" != none)

AC_ARG_ENABLE(autotest,
[AS_HELP_STRING([--enable-autotest],[Enable autotest testcases; breaks: make distcheck; default=disabled])],
[
    case "$enableval" in
	"yes") ENAB=yes ;;
	"no")  ENAB=no  ;;
	*) ENAB=no ;;
    esac
],
[
    ENAB=no
])
AM_CONDITIONAL(DO_AUTOTEST, test "$ENAB" = yes)

AC_ARG_WITH(testftpurl,
[AS_HELP_STRING([--with-testftpurl],[Enable test of FTP upload by giving a destination URL; default=disabled])],
[
    case "$withval" in
	"yes") WITHTESTFTPURL="ftp://anonymous@example.com/incoming/" ;;
	"no")  WITHTESTFTPURL=no  ;;
	*) WITHTESTFTPURL="$withval" ;;
    esac
],
[
    WITHTESTFTPURL=no
])
AM_CONDITIONAL(DO_TESTFTPURL, test "$WITHTESTFTPURL" != no)
AC_SUBST([WITHTESTFTPURL], ${WITHTESTFTPURL})

AC_CONFIG_FILES(Makefile
  java-pre/Makefile
  maildropqueue/Makefile maildropqueue/MailDropQueue.java
  tests-autotest/Makefile tests-autotest/atlocal tests-autotest/testsuite.at
)

if test "x$DOXYGEN" != "xmissing"
then
  AC_CONFIG_FILES(Doxyfile:pkg/Doxyfile.in)
fi

AC_OUTPUT
