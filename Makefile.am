
lib_LIBRARIES = libphenom.a

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libphenom.pc

ck_SOURCES = \
	thirdparty/ck/src/ck_bag.c \
	thirdparty/ck/src/ck_barrier_centralized.c \
	thirdparty/ck/src/ck_barrier_combining.c \
	thirdparty/ck/src/ck_barrier_dissemination.c \
	thirdparty/ck/src/ck_barrier_mcs.c \
	thirdparty/ck/src/ck_barrier_tournament.c \
	thirdparty/ck/src/ck_epoch.c \
	thirdparty/ck/src/ck_hp.c \
	thirdparty/ck/src/ck_hs.c \
	thirdparty/ck/src/ck_ht.c

libphenom_a_CPPFLAGS = \
	-Ithirdparty/ck/include -Iinclude \
	-DPHENOM_IMPL=1
libphenom_a_CFLAGS = @IRONMANCFLAGS@
libphenom_a_SOURCES = \
	corelib/init.c \
	corelib/buf.c \
	corelib/config.c \
	corelib/counter.c \
	corelib/debug_console.c \
	corelib/dns/addrinfo.c \
	corelib/dns/query.c \
	corelib/dtoa.c \
	corelib/error.c \
	corelib/hook.c \
	corelib/log.c \
	corelib/memory.c \
	corelib/pingfd.c \
	corelib/pipe2.c \
	corelib/pprintf.c \
	corelib/nbio.c \
	corelib/job.c \
	corelib/string.c \
	corelib/net/listener.c \
	corelib/net/sockaddr.c \
	corelib/net/socket.c \
	corelib/thread.c \
	corelib/timerwheel.c \
	corelib/vprintf.c \
	corelib/variant/variant.c \
	corelib/variant/json-dump.c \
	corelib/variant/json-load.c \
	corelib/variant/pack.c \
	corelib/variant/path.c \
	corelib/hash/murmur.c \
	corelib/hash/table.c \
	corelib/streams/make.c \
	corelib/streams/read.c \
	corelib/streams/write.c \
	corelib/streams/fd.c \
	corelib/streams/string.c \
	corelib/streams/temp.c \
	$(ck_SOURCES)

# some automakes put include/phenom in here, which causes conflicts
# with system includes so we turn off default includes by default.
DEFAULT_INCLUDES =

noinst_LIBRARIES = libtap.a
libtap_a_CPPFLAGS = -Iinclude
libtap_a_SOURCES = thirdparty/tap/tap.c

TEST_CPPFLAGS = -Ithirdparty/tap -Ithirdparty/ck/include -Iinclude -DPHENOM_IMPL
TEST_LDADD = libphenom.a libtap.a
TESTS = tests/counter.t tests/memory.t tests/timer.t tests/printf.t \
				tests/iobasic.t tests/stream.t tests/tpool.t \
				tests/string.t \
				tests/hashtable.t \
				tests/sockaddr.t \
				tests/dns.t \
				tests/variant.t \
				tests/bench/iopipes.t
noinst_PROGRAMS = $(TESTS) $(EXAMPLES)

EXAMPLES = examples/echo

# generate a rule that we can use to ensure that
# the test programs are built
build-tests: $(TESTS)
.PHONY: build-tests docs clean-docs

all-local: docs
clean-local: clean-docs
install-exec-local: install-phenom-hdrs

install-phenom-hdrs:
	$(mkdir_p) "$(DESTDIR)$(includedir)/phenom"
	$(INSTALL_HEADER) include/phenom/*.h $(DESTDIR)$(includedir)/phenom
	$(INSTALL_HEADER) thirdparty/ck/include/ck_*.h $(DESTDIR)$(includedir)
	$(mkdir_p) "$(DESTDIR)$(includedir)/gcc"
	$(INSTALL_HEADER) thirdparty/ck/include/gcc/*.h $(DESTDIR)$(includedir)/gcc
	for a in arm ppc ppc64 sparcv9 x86 x86_64 ; do \
		$(mkdir_p) "$(DESTDIR)$(includedir)/gcc/$$a" ; \
		$(INSTALL_HEADER) thirdparty/ck/include/gcc/$$a/*.h $(DESTDIR)$(includedir)/gcc/$$a ; \
	done

clean-docs:
	-rm docs/declmap.js

docs: docs/declmap.js

update-www: docs
	test -d ../phenom-pages
	cp docs/* ../phenom-pages/

docs/declmap.js: mkdoc.php include/phenom/*.h README.markdown
	@PHP@ mkdoc.php

examples_echo_CPPFLAGS = $(TEST_CPPFLAGS)
examples_echo_LDADD = $(TEST_LDADD)

tests_counter_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_counter_t_LDADD = $(TEST_LDADD)

tests_memory_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_memory_t_LDADD = $(TEST_LDADD)

tests_timer_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_timer_t_LDADD = $(TEST_LDADD)

tests_printf_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_printf_t_LDADD = $(TEST_LDADD)

tests_iobasic_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_iobasic_t_LDADD = $(TEST_LDADD)

tests_hashtable_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_hashtable_t_LDADD = $(TEST_LDADD)

tests_sockaddr_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_sockaddr_t_LDADD = $(TEST_LDADD)

tests_dns_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_dns_t_LDADD = $(TEST_LDADD)

tests_stream_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_stream_t_LDADD = $(TEST_LDADD)

tests_string_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_string_t_LDADD = $(TEST_LDADD)

tests_tpool_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_tpool_t_LDADD = $(TEST_LDADD)

tests_variant_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_variant_t_LDADD = $(TEST_LDADD)

if HAVE_LIBEVENT
LIBEVENT=-levent
endif
tests_bench_iopipes_t_CPPFLAGS = $(TEST_CPPFLAGS)
tests_bench_iopipes_t_LDADD = $(TEST_LDADD) $(LIBEVENT)
