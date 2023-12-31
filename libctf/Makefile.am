## Process this file with automake to produce Makefile.in.
#
#   Copyright (C) 2019-2020 Free Software Foundation, Inc.
#
# This file is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; see the file COPYING.  If not see
# <http://www.gnu.org/licenses/>.
#

ACLOCAL_AMFLAGS = -I .. -I ../config -I ../bfd

AUTOMAKE_OPTIONS = foreign no-texinfo.tex

# This is where we get zlib from.  zlibdir is -L../zlib and zlibinc is
# -I../zlib, unless we were configured with --with-system-zlib, in which
# case both are empty.
ZLIB = @zlibdir@ -lz
ZFUZZERLIB = @zlibdir@ -lzfuzzer
ZLIBINC = @zlibinc@

INCDIR = $(srcdir)/../include
AM_CPPFLAGS = -D_GNU_SOURCE -I$(srcdir) -I$(srcdir)/../include -I$(srcdir)/../bfd -I../bfd
AM_CFLAGS = -std=gnu99 @ac_libctf_warn_cflags@ @warn@ @c_warn@ @WARN_PEDANTIC@ @WERROR@ $(ZLIBINC)

if INSTALL_LIBBFD
lib_LTLIBRARIES = libctf@CTF_SOVER_EXT@.la libctf-nobfd@CTF_SOVER_EXT@.la libctf-nobfd-fuzzer@CTF_SOVER_EXT@.la 
include_HEADERS = $(INCDIR)/ctf.h $(INCDIR)/ctf-api.h
else
include_HEADERS =
noinst_LTLIBRARIES = libctf@CTF_SOVER_EXT@.la libctf-nobfd@CTF_SOVER_EXT@.la libctf-nobfd-fuzzer@CTF_SOVER_EXT@.la 
endif

COMMON_LIB_SOURCES = ctf-archive.c ctf-dump.c ctf-create.c ctf-decl.c ctf-error.c \
			  ctf-hash.c ctf-labels.c ctf-link.c ctf-lookup.c ctf-open.c \
			  ctf-string.c ctf-subr.c ctf-types.c ctf-util.c
if NEED_CTF_QSORT_R
COMMON_LIB_SOURCES += ctf-qsort_r.c
endif
libctf_nobfd@CTF_SOVER_EXT@_la_LIBADD = @SHARED_LIBADD@ $(ZLIB)
libctf_nobfd@CTF_SOVER_EXT@_la_LDFLAGS = -version-info 0:0:0 @SHARED_LDFLAGS@ @VERSION_FLAGS@
libctf_nobfd@CTF_SOVER_EXT@_la_SOURCES = $(COMMON_LIB_SOURCES)

libctf@CTF_SOVER_EXT@_la_LIBADD = @BFD_LIBADD@ $(libctf_nobfd@CTF_SOVER_EXT@_la_LIBADD)
libctf@CTF_SOVER_EXT@_la_DEPENDENCIES = @BFD_DEPENDENCIES@
libctf@CTF_SOVER_EXT@_la_LDFLAGS = $(libctf_nobfd@CTF_SOVER_EXT@_la_LDFLAGS)
libctf@CTF_SOVER_EXT@_la_SOURCES = $(COMMON_LIB_SOURCES) ctf-open-bfd.c


libctf_nobfd_fuzzer@CTF_SOVER_EXT@_la_LIBADD = @SHARED_LIBADD@ $(ZFUZZERLIB)
libctf_nobfd_fuzzer@CTF_SOVER_EXT@_la_CFLAGS =  -fsanitize=fuzzer-no-link  @SHARED_LDFLAGS@ @VERSION_FLAGS@
libctf_nobfd_fuzzer@CTF_SOVER_EXT@_la_SOURCES = $(COMMON_LIB_SOURCES)
