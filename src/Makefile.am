AM_CFLAGS = $(VARNISHAPI_CFLAGS) $(CODE_COVERAGE_CFLAGS) -I$(top_srcdir)/src/foreign \
	-Wall \
	-Werror \
	-Wall \
	-Wno-format-y2k \
	-Wstrict-prototypes \
	-Wmissing-prototypes \
	-Werror=missing-field-initializers \
	-Wpointer-arith \
	-Wreturn-type \
	-Wwrite-strings \
	-Wcast-qual \
	-Wswitch \
	-Wshadow \
	-Wunused-parameter \
	-Wcast-align \
	-Wchar-subscripts \
	-Wnested-externs \
	-Wextra \
	-Wno-sign-compare

AM_CPPFLAGS = $(CODE_COVERAGE_CPPFLAGS)
AM_LDFLAGS = $(VARNISHAPI_LIBS) $(VMOD_LDFLAGS) $(CODE_COVERAGE_LDFLAGS)

# Modules

vmod_LTLIBRARIES = \
	libvmod_accept.la \
	libvmod_bodyaccess.la \
	libvmod_header.la \
	libvmod_saintmode.la \
	libvmod_str.la \
	libvmod_tcp.la \
	libvmod_var.la \
	libvmod_vsthrottle.la \
	libvmod_xkey.la

generated_docs = $(dist_man_MANS:.3=.rst)

libvmod_accept_la_SOURCES = vmod_accept.c
libvmod_bodyaccess_la_SOURCES = vmod_bodyaccess.c
libvmod_header_la_SOURCES = vmod_header.c
libvmod_vsthrottle_la_SOURCES = vmod_vsthrottle.c
libvmod_saintmode_la_SOURCES = vmod_saintmode.c
libvmod_str_la_SOURCES = vmod_str.c
libvmod_tcp_la_SOURCES = vmod_tcp.c
libvmod_var_la_SOURCES = vmod_var.c
libvmod_xkey_la_SOURCES = vmod_xkey.c

nodist_libvmod_accept_la_SOURCES = vcc_accept_if.c vcc_accept_if.h
nodist_libvmod_bodyaccess_la_SOURCES = vcc_bodyaccess_if.c vcc_bodyaccess_if.h
nodist_libvmod_header_la_SOURCES = vcc_header_if.c vcc_header_if.h
nodist_libvmod_vsthrottle_la_SOURCES = vcc_vsthrottle_if.c vcc_vsthrottle_if.h
nodist_libvmod_saintmode_la_SOURCES = vcc_saintmode_if.c vcc_saintmode_if.h
nodist_libvmod_str_la_SOURCES = vcc_str_if.c vcc_str_if.h
nodist_libvmod_tcp_la_SOURCES = vcc_tcp_if.c vcc_tcp_if.h
nodist_libvmod_var_la_SOURCES = vcc_var_if.c vcc_var_if.h
nodist_libvmod_xkey_la_SOURCES = vcc_xkey_if.c vcc_xkey_if.h VSC_xkey.c VSC_xkey.h

vmod_xkey.3: VSC_xkey.rst

vmod_vcs_version.txt: $(nodist_libvmod_accept_la_SOURCES)

DISTCLEANFILES = vmod_vcs_version.txt

@BUILD_VMOD_ACCEPT@
@BUILD_VMOD_BODYACCESS@
@BUILD_VMOD_HEADER@
@BUILD_VMOD_SAINTMODE@
@BUILD_VMOD_STR@
@BUILD_VMOD_TCP@
@BUILD_VMOD_VAR@
@BUILD_VMOD_VSTHROTTLE@
@BUILD_VMOD_XKEY@
@BUILD_VSC_XKEY@

rst-docs: $(generated_docs)
	mkdir -p $(top_builddir)/docs/
	cp $(generated_docs) $(top_builddir)/docs/

doctest:
	for i in *.vcc; do \
		../test-vclsnippet $$i; \
		rst2html --halt=2 --exit-status=2 $$i 1>/dev/null; \
	done;

# Test suite

AM_TESTS_ENVIRONMENT = \
	PATH="$(abs_builddir):$(VARNISH_TEST_PATH):$(PATH)" \
	LD_LIBRARY_PATH="$(VARNISH_LIBRARY_PATH)"
TEST_EXTENSIONS = .vtc
# because of vsthrottle/test05.vtc we need larger buffers than usual
VTC_LOG_COMPILER = varnishtest -v -b 10M
AM_VTC_LOG_FLAGS = \
	-p vcl_path="$(abs_top_srcdir)/vcl:$(VARNISHAPI_VCLDIR)" \
	-p vmod_path="$(abs_builddir)/.libs:$(vmoddir):$(VARNISHAPI_VMODDIR)"

TESTS = \
	vtc/accept/test01.vtc \
	vtc/accept/test02.vtc \
	vtc/bodyaccess/test01.vtc \
	vtc/bodyaccess/test02.vtc \
	vtc/bodyaccess/test03.vtc \
	vtc/bodyaccess/test04.vtc \
	vtc/bodyaccess/test05.vtc \
	vtc/header/append.vtc \
	vtc/header/append_maxhdr_overflow.vtc \
	vtc/header/append_overflow.vtc \
	vtc/header/copy.vtc \
	vtc/header/get.vtc \
	vtc/header/import.vtc \
	vtc/header/keep-others.vtc \
	vtc/header/remove.vtc \
	vtc/header/some-data.vtc \
	vtc/header/regsub.vtc \
	vtc/saintmode/test01.vtc \
	vtc/saintmode/test02.vtc \
	vtc/saintmode/test03.vtc \
	vtc/saintmode/test04.vtc \
	vtc/saintmode/test05.vtc \
	vtc/str/test01.vtc \
	vtc/str/test02.vtc \
	vtc/str/test03.vtc \
	vtc/str/test04.vtc \
	vtc/str/test05.vtc \
	vtc/str/test06.vtc \
	vtc/str/test08.vtc \
	vtc/tcp/01-dumpinfo.vtc \
	vtc/tcp/02-congestion.vtc \
	vtc/tcp/03-read-tcpinfo.vtc \
	vtc/tcp/04-pacing.vtc \
	vtc/var/test01.vtc \
	vtc/var/test02.vtc \
	vtc/var/test03.vtc \
	vtc/var/test04.vtc \
	vtc/var/test05.vtc \
	vtc/var/test06.vtc \
	vtc/vsthrottle/test01.vtc \
	vtc/vsthrottle/test02.vtc \
	vtc/vsthrottle/test03.vtc \
	vtc/vsthrottle/test04.vtc \
	vtc/vsthrottle/test05.vtc \
	vtc/vsthrottle/test06.vtc \
	vtc/xkey/test01.vtc \
	vtc/xkey/test02.vtc \
	vtc/xkey/test03.vtc \
	vtc/xkey/test04.vtc \
	vtc/xkey/test05.vtc \
	vtc/xkey/test06.vtc \
	vtc/xkey/test07.vtc \
	vtc/xkey/test08.vtc \
	vtc/xkey/test09.vtc \
	vtc/xkey/test10.vtc \
	vtc/xkey/test11.vtc \
	vtc/xkey/test12.vtc \
	vtc/xkey/test13.vtc \
	vtc/xkey/test14.vtc

# Documentation

dist_doc_DATA = \
	foreign \
	vmod_config.h \
	$(TESTS) \
	vmod_accept.vcc \
	vmod_bodyaccess.vcc \
	vmod_header.vcc \
	vmod_saintmode.vcc \
	vmod_str.vcc \
	vmod_tcp.vcc \
	vmod_var.vcc \
	vmod_vsthrottle.vcc \
	vmod_xkey.vcc \
	xkey.vsc \
	vmod_vcs_version.txt

dist_man_MANS = \
	vmod_accept.3 \
	vmod_bodyaccess.3 \
	vmod_header.3 \
	vmod_saintmode.3 \
	vmod_str.3 \
	vmod_tcp.3 \
	vmod_var.3 \
	vmod_vsthrottle.3 \
	vmod_xkey.3

.rst.1:
	$(AM_V_GEN) $(RST2MAN) $< $@
