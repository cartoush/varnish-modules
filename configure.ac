AC_PREREQ([2.63])
AC_INIT([varnish-modules], [klarlack-0.24.0])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS(config.h)
AC_COPYRIGHT([Copyright (c) 2016-2021 Varnish Software Group])

AM_INIT_AUTOMAKE([1.12.2 foreign])
AM_SILENT_RULES([yes])
AM_EXTRA_RECURSIVE_TARGETS([rst-docs])

AC_ARG_VAR([RST2MAN], [the program to build mauals from reStructuredText])
AC_CHECK_PROGS(RST2MAN, [rst2man rst2man.py])

VARNISH_PREREQ([7.5.0])
VARNISH_VMODS([
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
VARNISH_COUNTERS([xkey])

AM_PROG_AR

LT_INIT([dlopen disable-static])
LT_PREREQ([2.2.6])

AC_CONFIG_FILES([
	Makefile
	src/Makefile
])

AC_OUTPUT

AX_PTHREAD(,[AC_MSG_ERROR([Could not configure pthreads support])])
AX_CODE_COVERAGE

save_CPPFLAGS=$CPPFLAGS
CPPFLAGS=$VARNISHAPI_CFLAGS

AC_CHECK_HEADERS([cache/cache_varnishd.h])

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

AS_ECHO("
	==== $PACKAGE_STRING ====

	varnish:      $VARNISH_VERSION
	prefix:       $prefix
	vmoddir:      $vmoddir
	vcldir:       $vcldir
	pkgvcldir:    $pkgvcldir

	compiler:     $CC
	cflags:       $CFLAGS
	ldflags:      $LDFLAGS
")
