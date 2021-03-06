include $(TOPDIR)/rules.mk

PKG_NAME:=frp
PKG_VERSION:=0.27.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/fatedier/frp/tar.gz/v${PKG_VERSION}?
PKG_HASH:=5d2efd5d924c7a7f84a9f2838de6ab9b7d5ca070ab243edd404a5ca80237607c

PKG_MAINTAINER:=Richard Yu <yurichard3839@gmail.com>
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/fatedier/frp
GO_PKG_BUILD_PKG:=github.com/fatedier/frp/cmd/...

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/frp/template
  SECTION:=net
  CATEGORY:=Network
  TITLE:=frp - fast reverse proxy
  URL:=https://github.com/fatedier/frp
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/frpc
  $(call Package/frp/template)
  TITLE+= (client)
endef

define Package/frps
  $(call Package/frp/template)
  TITLE+= (server)
endef

define Package/frp/description
  frp is a fast reverse proxy to help you expose a local server behind
  a NAT or firewall to the internet.
endef
Package/frpc/description = $(Package/frp/description)
Package/frps/description = $(Package/frp/description)

define Package/frp/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(2) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/frp
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/conf/$(2).ini $(1)/etc/frp/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/$(2).init $(1)/etc/init.d/$(2)
endef

define Package/frpc/install
  $(call Package/frp/install,$(1),frpc)
endef

define Package/frps/install
  $(call Package/frp/install,$(1),frps)
endef

$(eval $(call GoBinPackage,frpc))
$(eval $(call BuildPackage,frpc))
$(eval $(call GoBinPackage,frps))
$(eval $(call BuildPackage,frps))
