include $(TOPDIR)/rules.mk

PKG_NAME:=python-evdev
PKG_VERSION:=0.4.7
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=8d1cf6842c43f08ae3357019177884271c8d09ba
PKG_SOURCE_URL:=https://github.com/gvalkov/python-evdev.git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/python-evdev
  SECTION:=language-python
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=python-evdev
  URL:=python-evdev.readthedocs.org/en/latest
  DEPENDS:=+python
endef

define Package/python-evdev/description
Python bindings to the Linux input handling subsystem.
endef

define Build/Compile
	$(call Build/Compile/PyMod,,install --prefix=/usr --root=$(PKG_INSTALL_DIR))
endef

define Package/python-evdev/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* $(1)$(PYTHON_PKG_DIR)
endef

$(eval $(call BuildPackage,python-evdev))
