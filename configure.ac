AC_PREREQ([2.69])
AC_INIT([vinyl-modules], [klarlack-0.27.0])
AC_COPYRIGHT([Copyright (c) 2016-2024 Vinyl Software Group])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_SRCDIR(src/vmod_header.vcc)
AC_CONFIG_HEADERS(config.h)

AM_INIT_AUTOMAKE([1.12.2 -Wall -Werror foreign parallel-tests])
AM_SILENT_RULES([yes])
AM_EXTRA_RECURSIVE_TARGETS([rst-docs])

AC_ARG_VAR([RST2MAN], [the program to build manuals from reStructuredText])
AC_CHECK_PROGS(RST2MAN, [rst2man rst2man.py])

AC_ARG_WITH([lcov],
    AS_HELP_STRING(
        [--with-lcov=PATH],
        [Location of lcov to generate coverage data (auto)]),
    [LCOV="$withval"],
    [AC_CHECK_PROGS(LCOV, [lcov], [])])
AM_CONDITIONAL(HAVE_LCOV, [test -n "$LCOV"])

AC_ARG_WITH([genhtml],
    AS_HELP_STRING(
        [--with-genhtml=PATH],
        [Location of genhtml to generate coverage reports (auto)]),
    [GENHTML="$withval"],
    [AC_CHECK_PROGS(GENHTML, [genhtml], [])])
AM_CONDITIONAL(HAVE_GENHTML, [test -n "$GENHTML"])

VINYL_PREREQ([9.0.0])
VINYL_VMODS([
	accept
	bodyaccess
	header
	saintmode
	str
	tcp
	var
	vsthrottle
	xkey
])
VINYL_COUNTERS([xkey])

AM_PROG_AR

LT_PREREQ([2.2.6])
LT_INIT([dlopen disable-static])

AC_CONFIG_FILES([
	Makefile
	src/Makefile
])

AX_PTHREAD(,[AC_MSG_ERROR([Could not configure pthreads support])])
AX_CODE_COVERAGE

save_CPPFLAGS=$CPPFLAGS
CPPFLAGS=$VINYLAPI_CFLAGS

AC_CHECK_HEADERS([cache/cache_vinyld.h])

CPPFLAGS=$save_CPPFLAGS

AC_CHECK_TYPE([struct tcp_info],
	[AC_DEFINE([HAVE_TCP_INFO], [1],
		[Define if struct tcp_info is present])],
	[], [[
		#include <sys/types.h>
		#include <sys/socket.h>
		#include <netinet/tcp.h>
		#include <netinet/in.h>
	]])

AC_CHECK_PROG([HAVE_GIT], [git], [yes], [no])
if test "x$HAVE_GIT" = "xyes"; then
	if git rev-parse --git-dir >/dev/null 2>&1; then
		HAVE_GIT_REPO=yes
	else
		HAVE_GIT_REPO=no
	fi
else
	HAVE_GIT_REPO=no
fi
AM_CONDITIONAL([HAVE_VCS_VMOD_VERSION], [test -f "src/vmod_vcs_version.txt" || test "x$HAVE_GIT_REPO" = "xyes"])
if test "x$HAVE_GIT_REPO" != "xyes" && test ! -f "src/vmod_vcs_version.txt"; then
   AC_MSG_WARN([Building without version control metadata - vmod_vcs_version.txt will be unavailable])
fi

AC_OUTPUT

AS_ECHO("
	==== $PACKAGE_STRING ====

	vinyl:        $VINYL_VERSION
	prefix:       $prefix
	vmoddir:      $vmoddir
	vcldir:       $vcldir
	pkgvcldir:    $pkgvcldir

	compiler:     $CC
	cflags:       $CFLAGS
	ldflags:      $LDFLAGS
")
