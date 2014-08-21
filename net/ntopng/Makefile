include $(TOPDIR)/rules.mk

PKG_NAME:=ntopng
PKG_VERSION:=1.2.0
PKG_REVISION:=8116
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)_r$(PKG_REVISION).tgz
PKG_SOURCE_URL:=@SF/ntop
PKG_MD5SUM:=2d7468d5d46f3ca3f29ce3a093d826bf

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

CONFIGURE_ARGS+= \
	--target=$(GNU_TARGET_NAME) \
	--host=$(GNU_HOST_NAME) \
	--build=$(GNU_TARGET_NAME) \
	--prefix="$(PKG_INSTALL_DIR)/usr"

CONFIGURE_VARS+= \
	CPPFLAGS="-I$(STAGING_DIR)/usr/include/libxml2 $$$$CPPFLAGS"

TARGET_LDFLAGS+= \
	-lm

define Package/ntopng
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Web-based Traffic Analysis and Flow Collection
  DEPENDS:=+libxml2 +libsqlite3 +libpcap +libgeoip +libstdcpp +libjson-c +librt
  URL:=http://ntop.org/products/ntop/
endef

define Package/ntopng/description
	ntopng is the next generation version of the original ntop, a network traffic
	probe that shows the network usage, similar to what the popular top Unix command
	does. ntopng is based on libpcap.
endef

define Build/Configure
	( cd "$(PKG_BUILD_DIR)/nDPI"; autoreconf -ivf )
	( cd "$(PKG_BUILD_DIR)/nDPI/example/third-party/json-c"; autoreconf -ivf )
	( cd "$(PKG_BUILD_DIR)"; ./autogen.sh )
	$(call Build/Configure/Default)
	sed -e 's#g++#$$$$(CXX)#g' -i $(PKG_BUILD_DIR)/Makefile
	sed -e 's#\./configure;#\./configure --target=$(GNU_TARGET_NAME) --host=$(GNU_HOST_NAME) --build=$(GNU_TARGET_NAME);#g' -i $(PKG_BUILD_DIR)/Makefile
	sed -e 's#\./configure \(--[^t]\)#\./configure --target=$(GNU_TARGET_NAME) --host=$(GNU_HOST_NAME) --build=$(GNU_TARGET_NAME) \1#g' -i $(PKG_BUILD_DIR)/Makefile
	sed -e 's#\(cd \$$$$(LIBRRDTOOL_HOME);\) \(\./configure\)#\1 CPPFLAGS="-I$(STAGING_DIR)/usr/include/libxml2" \2#g' -i $(PKG_BUILD_DIR)/Makefile
	sed -e 's#-C src$$$$#-C src CFLAGS="-Os -pipe -fno-caller-saves -Wno-error=unused-but-set-variable" HOST_CC="gcc" CC="gcc" HOST_LDFLAGS="-L/lib/x86_64-linux-gnu -L/usr/lib/x86_64-linux-gnu"#g' -i $(PKG_BUILD_DIR)/third-party/LuaJIT-2.0.3/Makefile
	sed -e 's#^HOST_ALDFLAGS= .*$$$$#HOST_ALDFLAGS= $$$$(HOST_XLDFLAGS) $$$$(HOST_LDFLAGS)#g' -i $(PKG_BUILD_DIR)/third-party/LuaJIT-2.0.3/src/Makefile
endef

define Build/Compile
	$(call Build/Compile/Default,geoip)
	$(call Build/Compile/Default)
endef

define Package/ntopng/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/share/ntopng
	$(CP) -r $(PKG_INSTALL_DIR)/usr/share/$(PKG_NAME)/* $(1)/usr/share/$(PKG_NAME)
endef

$(eval $(call BuildPackage,ntopng))
