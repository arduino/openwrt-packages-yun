#
# Copyright (C) 2014 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=socket.io-client
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.9.17
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.nodejitsu.com/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=ad19f7d8ab8602ffd499938675cc238d

PKG_BUILD_DEPENDS:=node
PKG_NODE_VERSION:=0.10.28

include $(INCLUDE_DIR)/package.mk

define Package/node-socket.io-client
  DEPENDS:=+node +node-ws
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Socket.IO client for the browser and node.js
  URL:=https://www.npmjs.org/package/socket.io-client
endef

define Package/node-socket.io-client/description
 Socket.IO client for the browser and node.js
endef

define Build/Prepare
	/bin/tar xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR) --strip-components 1
	$(Build/Patch)
endef

define Build/Compile
	CC="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-gcc" \
	CXX="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	AR="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ar" \
	RANLIB="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-ranlib" \
	LINK="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	LD="$(TOOLCHAIN_DIR)/bin/mips-openwrt-linux-g++" \
	PATH="/usr/bin/:$(PATH)" \
	GYPFLAGS="-Dv8_use_mips_abi_hardfloat=false -Dv8_can_use_fpu_instructions=false" \
	LDFLAGS="-L$(TOOLCHAIN_DIR)/lib/ -Wl,-rpath-link $(TOOLCHAIN_DIR)/lib/" \
	npm_config_arch=mips \
	npm_config_nodedir=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/ \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install -g $(PKG_BUILD_DIR)
endef

define Package/node-socket.io-client/install
	mkdir -p $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules $(1)/usr/lib/
	rm -rf $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)/node_modules/ws
endef

$(eval $(call BuildPackage,node-socket.io-client))

