#
# Copyright (C) 2014 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=v8m-rb
PKG_VERSION:=3.12.14-git
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/paul99/v8m-rb.git
PKG_SOURCE_VERSION:=90a7f7b10aa2dc8a3a7f65f6092d68bffd729243
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_MD5SUM:=66a728a3cb8dad0e3773035c4049eb5f

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/v8m-rb
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Google v8 javascript engine
  URL:=https://github.com/paul99/v8m-rb/tree/dm-dev-mipsbe
  DEPENDS:=+libstdcpp +libc +libpthread
endef

define Package/v8m-rb/description
 Port of Google v8 javascript engine to MIPS architecture
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	(cd $(PKG_BUILD_DIR); make dependencies);
endef

define Build/Compile
	CC="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-gcc" \
	CXX="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	AR="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ar" \
	RANLIB="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ranlib" \
	LD="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ld" \
	LINK="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	PATH="/usr/bin/:$(PATH)" \
	GYPFLAGS="-Dv8_use_mips_abi_hardfloat=false -Dv8_can_use_fpu_instructions=false" \
	LDFLAGS="-L$(TOOLCHAIN_DIR)/lib/ -Wl,-rpath-link $(TOOLCHAIN_DIR)/lib/" \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)" mips.release library=shared snapshot=off
endef

define Package/v8m-rb/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/out/mips.release/lib.target/libv8.so $(1)/usr/lib/
endef

$(eval $(call BuildPackage,v8m-rb))
