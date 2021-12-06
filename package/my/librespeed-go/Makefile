# SPDX-Identifier-License: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=librespeed-go
PKG_VERSION:=1.1.4
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/librespeed/speedtest-go/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=409727df43c49119556f57b700a725e3bbec2a0f6ea54dfad948c5eefa715038

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILE:=LICENSE
PKG_MAINTAINER:=a <a@a>

PKG_CONFIG_DEPENDS:= \
	CONFIG_LIBRESPEED_GO_COMPRESS_GOPROXY \
	CONFIG_LIBRESPEED_GO_COMPRESS_UPX

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

UNPACK_CMD=gzip -dc $(DL_DIR)/$(PKG_SOURCE) | $(HOST_TAR) -C $(PKG_BUILD_DIR) $(TAR_OPTIONS)

GO_PKG:=github.com/librespeed/speedtest
GO_PKG_BUILD_PKG:=github.com/librespeed/speedtest
GO_PKG_LDFLAGS:=-s -w
COMPILE_TIME:= $(shell TZ=UTC-8 date '+%Y-%m-%d %H:%M:%S')
GO_PKG_LDFLAGS_X+= \
	$(GO_PKG).version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/librespeed-go/config
config LIBRESPEED_GO_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config LIBRESPEED_GO_COMPRESS_UPX
	bool "Compress executable files with UPX"
	default y
endef

ifeq ($(CONFIG_LIBRESPEED_GO_COMPRESS_GOPROXY),y)
	export GO111MODULE=on
	export GOPROXY=https://goproxy.io
endif

define Package/librespeed-go
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Librespeed (Golang)
  URL:=https://github.com/librespeed/speedtest-go
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Build/Prepare
	$(Build/Prepare/Default)
	cp -ar $(PKG_BUILD_DIR)/speedtest-go-$(PKG_VERSION)/* $(PKG_BUILD_DIR)/
	rm -rf $(PKG_BUILD_DIR)/speedtest-go-$(PKG_VERSION)
endef

define Build/Compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_GOST_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/speedtest
endif
endef

define Package/librespeed-go/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin/librespeed
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/speedtest $(1)/usr/bin/librespeed/librespeed
	$(INSTALL_DIR) $(1)/usr/bin/librespeed/assets
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./file/root/etc/init.d/librespeed-go $(1)/etc/init.d/librespeed-go
	sed -i 's;assets_path.*;assets_path="/usr/bin/librespeed/assets";' $(PKG_BUILD_DIR)/settings.toml
	$(CP) $(PKG_BUILD_DIR)/web/assets/* $(1)/usr/bin/librespeed/assets/
	$(CP) $(PKG_BUILD_DIR)/settings.toml $(1)/usr/bin/librespeed/
endef

$(eval $(call GoBinPackage,librespeed-go))
$(eval $(call BuildPackage,librespeed-go))
