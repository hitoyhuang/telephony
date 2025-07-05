#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel
PKG_VERSION:=20250324
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=3d45c7f072131296a7e3c1a4faf5bb18751dbd87
PKG_SOURCE_DATE=2025-03-24
PKG_MIRROR_HASH:=e2b0a469aa0256b7782a880109a0393612a49176b483dc4c97f4a20416464d4a

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE

PKG_FIXUP:=autoreconf
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

# 5G 支持定义
CONFIGURE_ARGS += \
    --enable-5g-nr \
    --enable-vonr \
    --with-5g-models="RM500Q,RM510Q,RG500U"

define Package/asterisk-chan-quectel
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Telephony
  TITLE:=Quectel 5G modem support for Asterisk
  DEPENDS:=+asterisk +libpthread +librt +kmod-usb-serial +kmod-usb-serial-option +kmod-usb-acm
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel
endef

define Package/asterisk-chan-quectel/description
  Channel driver for connecting Asterisk to Quectel 4G/5G modems.
  Supports VoLTE/VoNR and advanced 5G features.
endef

define Build/Configure
  (cd $(PKG_BUILD_DIR); \
   ./autogen.sh; \
   $(TARGET_CONFIGURE_OPTS) \
   ./configure \
        --host=$(GNU_TARGET_NAME) \
        --build=$(GNU_HOST_NAME) \
        --prefix=/usr \
        $(CONFIGURE_ARGS) \
  )
endef

define Package/asterisk-chan-quectel/install
	$(INSTALL_DIR) $(1)/usr/lib/asterisk/modules
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)/usr/lib/asterisk/modules/
	
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk/
	
	$(INSTALL_DIR) $(1)/usr/share/asterisk/quectel
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/sounds/*.gsm $(1)/usr/share/asterisk/quectel/
endef

$(eval $(call BuildPackage,asterisk-chan-quectel))
