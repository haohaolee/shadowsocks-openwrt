include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocks-libev
PKG_VERSION:=2013-1-1
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/haohaolee/shadowsocks-libev.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=2493de62d3b4ef5f512d930a9e84f8e14b8156d8
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

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include -I$(STAGING_DIR)/usr/include/libev -lm
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib/libev -L$(STAGING_DIR)/usr/lib

CONFIGURE_ARGS+= \
		ac_cv_search_ev_io_start="-Wl,-static -lev" \
        ac_cv_search_MD5="-Wl,-Bdynamic -lcrypto"

define Package/shadowsocks-libev/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/local $(1)/usr/bin/shadowsocks
	$(INSTALL_BIN) ./files/shadowsocks.init $(1)/etc/init.d/shadowsocks
	$(INSTALL_CONF) ./files/shadowsocks.config $(1)/etc/config/shadowsocks
endef

$(eval $(call BuildPackage,shadowsocks-libev))

