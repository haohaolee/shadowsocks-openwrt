include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocks-libev
PKG_VERSION:=2013-1-10
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/haohaolee/shadowsocks-libev.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=bcf357ba459c1b7f9bc6701ecb7e5cee6f9f19fc
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MAINTAINER:=haohaolee <hayatelee@gmail.com>
# PKG_MIRROR_MD5SUM:=

PKG_BUILD_DEPENDS:=libev
PKG_INSTALL:=1 
PKG_FIXUP:=autoreconf 

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/shadowsocks-libev
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libopenssl
  TITLE:=shadowsocks implementation with libev
endef

define Package/shadowsocks-libev/conffiles
/etc/config/shadowsocks
endef

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include -I$(STAGING_DIR)/usr/include/libev
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib/libev -L$(STAGING_DIR)/usr/lib -lm

CONFIGURE_ARGS+= \
		ac_cv_search_ev_io_start="-Wl,-static,-lev -Wl,-Bdynamic"

define Package/shadowsocks-libev/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ss $(1)/usr/bin/shadowsocks
	$(INSTALL_BIN) ./files/shadowsocks.init $(1)/etc/init.d/shadowsocks
	$(INSTALL_CONF) ./files/shadowsocks.config $(1)/etc/config/shadowsocks
endef

$(eval $(call BuildPackage,shadowsocks-libev))

