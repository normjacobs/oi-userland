#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#
# Copyright (c) 2011, 2013, Oracle and/or its affiliates. All rights reserved.
# Copyright (c) 2018 Norm Jacobs
#
include ../../../make-rules/shared-macros.mk

COMPONENT_NAME=		binutils
COMPONENT_VERSION=	2.30
COMPONENT_REVISION=	0
COMPONENT_PROJECT_URL=	http://www.gnu.org/software/binutils/
COMPONENT_SRC=		$(COMPONENT_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.bz2
COMPONENT_ARCHIVE_URL=	http://ftp.gnu.org/gnu/$(COMPONENT_NAME)/$(COMPONENT_ARCHIVE)
COMPONENT_ARCHIVE_HASH=	\
	sha256:efeade848067e9a03f1918b1da0d37aaffa0b0127a06b5e9236229851d9d0c09
COMPONENT_BUGDB=	utility/gnu-binutils

include $(WS_TOP)/make-rules/prep.mk
include $(WS_TOP)/make-rules/configure.mk
include $(WS_TOP)/make-rules/ips.mk

PATH=/usr/gnu/bin:/usr/bin

define add-target

CONFIGURE_TARGET_$(1) =	$(BUILD_DIR)/$(MACH)/$(1)/.configured
BUILD_TARGET_$(1) =	$(BUILD_DIR)/$(MACH)/$(1)/.built
INSTALL_TARGET_$(1) =	$(BUILD_DIR)/$(MACH)/$(1)/.installed
TEST_TARGET_$(1) =	$(BUILD_DIR)/$(MACH)/$(1)/.tested


ifneq ($(findstring $(1),$(GNU_TARGET_TRIPLE.i386.32) $(GNU_TARGET_TRIPLE.sparc.32)),)
$$(CONFIGURE_TARGET_$(1)):	BITS=32
else
$$(CONFIGURE_TARGET_$(1)):	BITS=64
endif
$$(CONFIGURE_TARGET_$(1)):	GNU_TARGET_TRIPLE=$(1)
$$(BUILD_TARGET_$(1)):		GNU_TARGET_TRIPLE=$(1)
$$(INSTALL_TARGET_$(1)):	GNU_TARGET_TRIPLE=$(1)
$$(INSTALL_TARGET_$(1)):	PROTO_DIR=$(BUILD_DIR)/prototype/$(MACH)/$(1)
$$(TEST_TARGET_$(1)):		GNU_TARGET_TRIPLE=$(1)
$(BUILD_DIR)/manifest-$(MACH)-binutils-$(1).mogrified: GNU_TARGET_TRIPLE=$(1)

CONFIGURE_TARGETS +=	$$(CONFIGURE_TARGET_$(1))
BUILD_TARGETS +=	$$(BUILD_TARGET_$(1))
INSTALL_TARGETS +=	$$(INSTALL_TARGET_$(1))
TEST_TARGETS +=		$$(TEST_TARGET_$(1))

endef

# Various Platform support
TARGETS +=	$(GNU_TARGET_TRIPLE.i386.32)
TARGETS +=	$(GNU_TARGET_TRIPLE.sparc.32)
TARGETS +=	$(GNU_TARGET_TRIPLE.i386.64)
TARGETS +=	$(GNU_TARGET_TRIPLE.sparc.64)
TARGETS +=	$(GNU_TARGET_TRIPLE.arm.64)

# Add rules for each target
$(foreach target, $(TARGETS), $(eval $(call add-target,$(target))))

# default (for main package)
GNU_TARGET_TRIPLE =		$(GNU_TARGET_TRIPLE.$(MACH).32)

PKG_MACROS +=	GNU_TARGET_TRIPLE="$(GNU_TARGET_TRIPLE)"
# 64-bit (for main package)
PKG_MACROS +=	GNU_TARGET_TRIPLE.64="$(GNU_TARGET_TRIPLE.$(MACH).64)"

CONFIGURE_PREFIX =	/usr/gnu

CONFIGURE_OPTIONS +=	--mandir=$(CONFIGURE_MANDIR)
CONFIGURE_OPTIONS +=	--infodir=$(CONFIGURE_INFODIR)
CONFIGURE_OPTIONS +=	--enable-64-bit-bfd
CONFIGURE_OPTIONS +=    --enable-gold=no
CONFIGURE_OPTIONS +=    --enable-plugins=no
CONFIGURE_OPTIONS +=    --enable-nls
CONFIGURE_OPTIONS +=    --disable-libtool-lock
CONFIGURE_OPTIONS +=    --enable-largefile=yes
CONFIGURE_OPTIONS +=	--with-system-zlib
CONFIGURE_OPTIONS +=	--with-sysroot
CONFIGURE_OPTIONS +=    --target=$(GNU_TARGET_TRIPLE)
CONFIGURE_OPTIONS +=	--program-transform-name='s,^,$(GNU_TARGET_TRIPLE)-,'

CONFIGURE_BINDIR.64 = $(CONFIGURE_BINDIR.32)
CONFIGURE_LIBDIR.64 = $(CONFIGURE_LIBDIR.32)


# common targets
configure: $(CONFIGURE_TARGETS)

build:		$(BUILD_TARGETS)

install:	$(INSTALL_TARGETS)

test: $(TEST_TARGETS)

REQUIRED_PACKAGES += library/zlib
REQUIRED_PACKAGES += system/library
