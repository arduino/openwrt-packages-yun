#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=bluez
PKG_VERSION:=5.13
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.kernel.org/pub/linux/bluetooth
PKG_MD5SUM:=1a2b9ada9bd6542f6efee413aed280d9

PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/bluez/Default
  URL:=http://www.bluez.org/
endef

define Package/bluez-utils
$(call Package/bluez/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Bluetooth utilities
  DEPENDS:=+bluez-libs
endef

define Package/bluez-libs
$(call Package/bluez/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Bluetooth libraries
  DEPENDS:=+libpthread +libusb +glib2 +dbus +udev $(INTL_DEPENDS) $(ICONV_DEPENDS)
endef

CONFIGURE_ARGS += \
	--disable-systemd \
	--disable-monitor \
	--disable-cups \
	--disable-client \
	--disable-obex \
	--enable-library \
	--with-udevdir="$(STAGING_DIR)/lib/udev"

TARGET_CFLAGS += $(FPIC)

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
	LDFLAGS="$(TARGET_LDFLAGS) \
	-L$(ICONV_PREFIX)/lib \
	-L$(INTL_PREFIX)/lib -lm" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	all install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/bluetooth
	$(CP) $(PKG_INSTALL_DIR)/usr/include/bluetooth/*.h $(1)/usr/include/bluetooth
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/bluez.pc $(1)/usr/lib/pkgconfig/
endef

define Package/bluez-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
endef

define Package/bluez-libs/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/lib/udev/rules.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)$(STAGING_DIR)/lib/udev/hid2hci $(1)/lib/udev
	$(CP) $(PKG_INSTALL_DIR)$(STAGING_DIR)/lib/udev/rules.d/* $(1)/lib/udev/rules.d/
endef

$(eval $(call BuildPackage,bluez-utils))
$(eval $(call BuildPackage,bluez-libs))

