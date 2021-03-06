# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=lwm2m-node-lib
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=1.3.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://mirrors.cloud.tencent.com/npm/$(PKG_NPM_NAME)/-/ \
                https://registry.npm.taobao.org/$(PKG_NPM_NAME)/-/ \
                https://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_HASH:=8982693429b014bb2cd93f7b93f8521a274ec5d7afeadae505b5c49a0fc16306

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=AGPL-3.0
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-lwm2m-node-lib
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Library for building OMA Lightweight M2M
  URL:=https://www.npmjs.org/package/lwm2m-node-lib
  DEPENDS:=+node
endef

define Package/node-lwm2m-node-lib/description
 The Open Mobile Alliance Lightweight M2M protocol is a machine to machine communication protocol built over COAP, and meant to communicate resource constrained devices. The protocol defines two roles for the devices: a Lightweight M2M Client (the constrained device) and a Lightweight M2M Server (meant to consume the device data and control its execution).
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Prepare
	$(INSTALL_DIR) $(PKG_BUILD_DIR)
endef

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install -g $(DL_DIR)/$(PKG_SOURCE)
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Package/node-lwm2m-node-lib/install
	$(INSTALL_DIR) $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/lwm2m-node-lib/bin/iotagent-lwm2m-client.js $(1)/usr/bin/iotagent-lwm2m-client
	$(LN) ../lib/node/lwm2m-node-lib/bin/iotagent-lwm2m-server.js $(1)/usr/bin/iotagent-lwm2m-server
endef

$(eval $(call BuildPackage,node-lwm2m-node-lib))
