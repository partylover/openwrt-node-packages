#
# Copyright (C) 2014-2016 Arduino LLC
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=socket.io
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=1.4.5
PKG_RELEASE:=3

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=ff92f4478096a350086ec1c77d60a63e

PKG_BUILD_DEPENDS:=node/host
PKG_NODE_VERSION:=4.4.3
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-socket.io
  DEPENDS:=+node +node-socket.io-client
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Real-time apps made cross-browser & easy with a WebSocket-like API
  URL:=https://www.npmjs.org/package/socket.io
endef

define Package/node-socket.io/description
 Real-time apps made cross-browser & easy with a WebSocket-like API
endef

CPU:=$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))

define Build/Prepare
	/bin/tar xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR) --strip-components 1
	$(Build/Patch)
endef

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CONFIG_ARCH) \
	npm_config_nodedir=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/ \
	npm_config_cache=$(BUILD_DIR)/node-v$(PKG_NODE_VERSION)/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	$(STAGING_DIR)/host/bin/npm install --target_arch=$(CPU) -g $(PKG_BUILD_DIR)
endef

define Package/node-socket.io/install
	mkdir -p $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
	rm -rf $(1)/usr/lib/node/$(PKG_NPM_NAME)/node_modules/socket.io-client
endef

$(eval $(call BuildPackage,node-socket.io))
