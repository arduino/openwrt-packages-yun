# 
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
PKG_NAME:=yun-gcc
PKG_VERSION:=4.6.2
PKG_RELEASE:=2
PATCH_DIR=$(TOPDIR)/feeds/packages/devel/yun-gcc/patches/
PKG_SOURCE:=gcc-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.fu-berlin.de/unix/languages/gcc/releases/gcc-$(PKG_VERSION) \
	http://www.netgull.com/gcc/releases/gcc-$(PKG_VERSION) \
	ftp://ftp.gnu.org/gnu/gcc/gcc-$(PKG_VERSION)
PKG_MD5SUM:=028115c4fbfb6cfd75d6369f4a90d87e
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

# There should be a depend that prevents this being used if not compiling for a Yun
# Should we depend on binutils?
include $(INCLUDE_DIR)/package.mk
RSTRIP := true
define Package/yun-gcc
  SECTION:=devel
  CATEGORY:=Development
  TITLE:=native gcc for the Arduino yun
  DEPENDS:= +libpthread 
endef

define Package/yun-gcc/description
	build a native toolchain for compiling on target
	please see this http://playground.arduino.cc/Hardware/Yun#Compiling_on_the_Yun before use
endef

# The link for cc should really be done in the toolchain installation, not here.
define Build/Prepare 
	/bin/tar xjf $(DL_DIR)/$(PKG_SOURCE)  -C $(BUILD_DIR)
	mv $(BUILD_DIR)/gcc-$(PKG_VERSION)/* $(PKG_BUILD_DIR)
	ln -sf $(STAGING_DIR)/bin/mips-openwrt-linux-gcc  \
		$(STAGING_DIR)/bin/mips-openwrt-linux-cc
	cd $(PKG_BUILD_DIR);$(PKG_BUILD_DIR)/contrib/download_prerequisites
	$(Build/Patch)
endef

#TODO change this to use OpenWrt's normal configure procedure	
#TODO change this to use the normal ipkg installation directory
define Build/Configure	
	rm -rf $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir 
	mkdir $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir
	cd $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir;\
	CFLAGS="-g -O2";CFLAGS_FOR_TARGET="-g -O2";\
	../$(PKG_NAME)-$(PKG_VERSION)/configure \
		--host=mips-openwrt-linux-uclibc \
		--target=mips-openwrt-linux-uclibc \
		--prefix="$(STAGING_DIR)/yun-gcc" \
		--disable-subdir-texinfo MAKEINFO=missing \
		--disable-decimal-float \
		--disable-libquadmath \
		--disable-lto \
		--disable-libgomp \
		--disable-libmudflap \
		--disable-multilib \
		--disable-nls \
		--with-float=soft \
		--enable-libstdcxx-time
		--enable-languages=c,c++ 
endef


		
#this is a better place to install from 
#DESTDIR="$(PKG_INSTALL_DIR)" $(MAKE_ARGS) all install
#OpenWrt sets environment variables that kill the build so we use env to 
#get a clean state and then set only what is needed
define Build/Compile
	cd $(PKG_BUILD_DIR)/../$(PKG_NAME)-$(PKG_VERSION)objdir;\
		env -i - PATH=$(PATH) STAGING_DIR=$(STAGING_DIR) make;\
		make install
endef

# TODO While we can't strip some of the libraries much of this can be stripped,
# TODO selectively stripping the things that can be would reduce space usage. 
define Package/yun-gcc/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/libexec/
	$(INSTALL_DIR) $(1)/usr/include/
	(cd $(STAGING_DIR)/yun-gcc/lib/gcc/mips-openwrt-linux-uclibc/4.6.2/;\
	ln -s -f ../4.6.3/crtbegin.o crtbegin.o;\
	ln -s -f ../4.6.3/crtbeginS.o crtbeginS.o;\
	ln -s -f ../4.6.3/crtbeginT.o crtbeginT.o;\
	ln -s -f ../4.6.3/crtend.o crtend.o;\
	ln -s -f ../4.6.3/crtendS.o crtendS.o;\
	ln -s -f ../4.6.3/crtfastmath.o crtfastmath.o;\
	ln -s -f ../4.6.3/libgcc.a libgcc.a;\
	ln -s -f ../4.6.3/libgcc_eh.a libgcc_eh.a;\
	ln -s -f ../4.6.3/libgcov.a libgcov.a);
	

	cp -r $(STAGING_DIR)/yun-gcc/bin/* $(1)/usr/bin/
	find $(TOOLCHAIN_DIR)/lib/ -maxdepth 1 -type f | xargs -I {} cp {} $(1)/usr/lib/
	cp -r $(STAGING_DIR)/yun-gcc/libexec/* $(1)/usr/libexec/
	cp -r $(STAGING_DIR)/yun-gcc/lib/* $(1)/usr/lib/
	cp -r $(TOPDIR)/staging_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/lib/* $(1)/usr/lib/
	cp -r $(TOPDIR)/staging_dir/target-mips_r2_uClibc-0.9.33.2/usr/lib/libusbpp.a $(1)/usr/lib/
	cp -r $(TOPDIR)/build_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/uClibc-dev/include/* $(1)/usr/include/
	cp -r $(TOPDIR)/build_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/gcc-linaro-4.6-2012.02/libstdc++-v3/include/* $(1)/usr/include/
	cp -r $(TOPDIR)/staging_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/mips-openwrt-linux-uclibc/include/c++/4.6.3/mips-openwrt-linux-uclibc/* $(1)/usr/include/
	cp -r $(TOPDIR)/staging_dir/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/mips-openwrt-linux-uclibc/include/c++/4.6.3/* $(1)/usr/include/
	cp -r $(TOPDIR)/staging_dir/target-mips_r2_uClibc-0.9.33.2/usr/include/*  $(1)/usr/include/

endef

$(eval $(call BuildPackage,yun-gcc))
