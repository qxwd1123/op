include $(TOPDIR)/rules.mk

PKG_NAME:=subconverter
PKG_VERSION:=0.7.2
PKG_RELEASE:=$(AUTORELEASE)

PKG_LICENSE:=GPF
PKG_MAINTAINER:=

PKG_SOURCE:=subconverter_linux64.tar.gz
PKG_SOURCE_URL:=https://github.com/tindy2013/subconverter/releases/download/v$(PKG_VERSION)/
PKG_HASH:=skip

include $(INCLUDE_DIR)/package.mk

define Package/subconverter
    SECTION:=
    CATEGORY:=
    DEPENDS:=
    TITLE:=Utility to convert between various subscription format
    URL:=https://github.com/tindy2013/subconverter
endef

define Package/subconverter/description
  Utility to convert between various subscription format.
endef

define Package/subconverter/conffiles
/etc/subconverter
endef

define Build/Compile
	echo "subconverter using precompiled binary."
endef

define Package/subconverter/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/subconverter $(1)/usr/bin/subconverter
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/aliyundrive-fuse.init $(1)/etc/init.d/subconverter
endef

$(eval $(call BuildPackage,subconverter))
