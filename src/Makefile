#
# Copyright (c) 2000-2005 Silicon Graphics, Inc.  All Rights Reserved.
#

TOPDIR = ..
include $(TOPDIR)/include/builddefs

TARGETS = dirstress fill fill2 getpagesize holes lstat64 \
	nametest permname randholes runas truncfile usemem \
	mmapcat append_reader append_writer dirperf metaperf \
	devzero feature alloc fault fstest t_access_root \
	godown resvtest writemod makeextents itrash rename \
	multi_open_unlink dmiperf unwritten_sync genhashnames t_holes \
	t_mmap_writev t_truncate_cmtime dirhash_collide

LINUX_TARGETS = xfsctl bstat t_mtab getdevicesize preallo_rw_pattern_reader \
	preallo_rw_pattern_writer ftrunc trunc fs_perms testx looptest \
	locktest unwritten_mmap bulkstat_unlink_test t_stripealign \
	bulkstat_unlink_test_modified t_dir_offset t_futimens t_immutable \
	stale_handle pwrite_mmap_blocked t_dir_offset2 seek_sanity_test \
	seek_copy_test t_readdir_1 t_readdir_2 fsync-tester nsexec cloner \
	renameat2 t_getcwd e4compact

SUBDIRS =

LLDLIBS = $(LIBATTR) $(LIBHANDLE) $(LIBACL)

ifeq ($(HAVE_XLOG_ASSIGN_LSN), true)
LINUX_TARGETS += loggen
endif

ifeq ($(HAVE_FIEMAP), true)
LINUX_TARGETS += fiemap-tester
endif

ifeq ($(HAVE_FALLOCATE), true)
LCFLAGS += -DHAVE_FALLOCATE
endif

IRIX_TARGETS = open_unlink

ifeq ($(PKG_PLATFORM),linux)
TARGETS += $(LINUX_TARGETS)
endif

ifeq ($(PKG_PLATFORM),irix)
TARGETS += $(IRIX_TARGETS)
LLDLIBS += -lgen
endif

ifeq ($(HAVE_DB), true)
TARGETS += dbtest
LLDLIBS += $(LIBGDBM)
endif

ifeq ($(HAVE_AIO), true)
SUBDIRS += aio-dio-regress
endif

ifeq ($(HAVE_SSL), true)
TARGETS += fssum
LLDLIBS += -lssl -lcrypto
ifeq ($(PKG_PLATFORM),linux)
CFLAGS += -D__LINUX__
endif
endif

CFILES = $(TARGETS:=.c)
LDIRT = $(TARGETS)


default: depend $(TARGETS) $(SUBDIRS)

depend: .dep

include $(BUILDRULES)

$(TARGETS): $(LIBTEST)
	@echo "    [CC]    $@"
	$(Q)$(LTLINK) $@.c -o $@ $(CFLAGS) $(LDFLAGS) $(LDLIBS) $(LIBTEST)

LINKTEST = $(LTLINK) $@.c -o $@ $(CFLAGS) $(LDFLAGS)

install: default $(addsuffix -install,$(SUBDIRS))
	$(INSTALL) -m 755 -d $(PKG_LIB_DIR)/src
	$(LTINSTALL) -m 755 $(TARGETS) $(PKG_LIB_DIR)/src
	$(LTINSTALL) -m 755 fill2attr fill2fs fill2fs_check scaleread.sh $(PKG_LIB_DIR)/src
	$(LTINSTALL) -m 644 dumpfile $(PKG_LIB_DIR)/src

%-install:
	$(MAKE) -C $* install

-include .dep
