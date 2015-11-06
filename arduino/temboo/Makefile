#
# Copyright (c) 2013 Arduino LLC. All right reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=temboo
PKG_VERSION:=1.2.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://arduino.cc/download.php?f=/
PKG_MD5SUM:=5ca4ba62e6b94f2f06959b1ad7eb65a4

include $(INCLUDE_DIR)/package.mk

define Package/temboo
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Build Inventive Apps With Ease
  DEPENDS:=+python-mini
endef

define Package/temboo/description
	1500+ cloud-based code components that let you create without limits.
endef

define Build/Compile
	# NOOP
endef

define Package/temboo/install
	$(INSTALL_DIR) $(1)/usr/bin

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/client/temboo $(1)/usr/bin
	cp -r $(PKG_BUILD_DIR)/client/temboo-m2m $(1)/usr/bin
endef

$(eval $(call BuildPackage,temboo))

