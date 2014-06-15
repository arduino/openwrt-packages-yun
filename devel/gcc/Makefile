# 
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
PKG_NAME:=gcc
PKG_VERSION:=4.6.2
PKG_RELEASE:=1
PATCH_DIR=$(TOPDIR)/feeds/packages/devel/gcc/patches/
PKG_SOURCE_URL:=ftp://ftp.fu-berlin.de/unix/languages/gcc/releases/gcc-$(PKG_VERSION) \
	http://www.netgull.com/gcc/releases/gcc-$(PKG_VERSION) \
	ftp://ftp.gnu.org/gnu/gcc/gcc-$(PKG_VERSION)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_MD5SUM:=028115c4fbfb6cfd75d6369f4a90d87e
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

# There should be a depend that prevents this being used if not compiling for a Yun
include $(INCLUDE_DIR)/package.mk
RSTRIP := true
define Package/gcc
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=native gcc
  DEPENDS:= +libpthread 
endef

define Package/gcc/description
	build a native toolchain for compiling on target
endef

define Build/Prepare
	$(call Build/Prepare/Default)
# This should really be done in the toolchain installation, not here.
	ln -sf $(STAGING_DIR)/bin/mips-openwrt-linux-gcc  \
		$(STAGING_DIR)/bin/mips-openwrt-linux-cc
	cd $(PKG_BUILD_DIR);$(PKG_BUILD_DIR)/contrib/download_prerequisites
# Since OpenWrt has already applied patches we have to do this one ourself
	cd $(PKG_BUILD_DIR)/mpfr-2.4.2;cp $(TOPDIR)/feeds/packages/devel/gcc/mpfrpatch ./mpfrpatch;patch mpfr-longlong.h < mpfrpatch

endef

define Build/Configure	
#TODO change this to use OpenWrt's normal configure procedure	
	(mkdir $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir;cd $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir;CFLAGS="-g -O2";CFLAGS_FOR_TARGET="-g -O2";../$(PKG_NAME)-$(PKG_VERSION)/configure \
		--host=mips-openwrt-linux-uclibc \
		--target=mips-openwrt-linux-uclibc \
		--prefix="$(STAGING_DIR)/nativeGcc" \
		--disable-subdir-texinfo MAKEINFO=missing \
		--disable-decimal-float \
		--disable-libquadmath \
		--disable-lto \
		--enable-languages=c,c++ );
endef


define Build/Compile
	cd $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir;\
		env -i - PATH=$(PATH) STAGING_DIR=$(STAGING_DIR) make;\
		make install
		
#this is a better place to install from 
#DESTDIR="$(PKG_INSTALL_DIR)" $(MAKE_ARGS) all install
endef

define Package/gcc/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/libexec/
	$(INSTALL_DIR) $(1)/usr/include/
	(cd $(STAGING_DIR)/nativeGcc/lib/gcc/mips-openwrt-linux-uclibc/4.6.2/;\
	ln -s -f ../4.6.3/crtbegin.o crtbegin.o;\
	ln -s -f ../4.6.3/crtbeginS.o crtbeginS.o;\
	ln -s -f ../4.6.3/crtbeginT.o crtbeginT.o;\
	ln -s -f ../4.6.3/crtend.o crtend.o;\
	ln -s -f ../4.6.3/crtendS.o crtendS.o;\
	ln -s -f ../4.6.3/crtfastmath.o crtfastmath.o;\
	ln -s -f ../4.6.3/libgcc.a libgcc.a;\
	ln -s -f ../4.6.3/libgcc_eh.a libgcc_eh.a;\
	ln -s -f ../4.6.3/libgcov.a libgcov.a);
	

	cp -r $(STAGING_DIR)/nativeGcc/bin/* $(1)/usr/bin/
	find $(TOOLCHAIN_DIR)/lib/ -maxdepth 1 -type f | xargs -I {} cp {} $(1)/usr/lib/
	cp -r $(STAGING_DIR)/nativeGcc/libexec/* $(1)/usr/libexec/
	cp -r $(STAGING_DIR)/nativeGcc/lib/* $(1)/usr/lib/
	cp -r $(TOPDIR)/staging_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/lib/* $(1)/usr/lib/
	cp $(TOPDIR)/build_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/gcc-linaro-4.6-2012.02/fixincludes/tests/base/stdio.h $(1)/usr/include/

endef

$(eval $(call BuildPackage,gcc))
