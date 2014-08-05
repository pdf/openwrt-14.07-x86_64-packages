#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gnupg
PKG_VERSION:=1.4.16
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.franken.de/pub/crypt/mirror/ftp.gnupg.org/gcrypt/gnupg \
	ftp://ftp.gnupg.org/gcrypt/gnupg
PKG_MD5SUM:=6df73c57d3ece1dd36dc2a7679f00fb0

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/gnupg
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+zlib +libncurses +libreadline +libopenldap +bzip2 +libcurl
  TITLE:=GNU privacy guard - a free PGP replacement (big version)
  URL:=http://www.gnupg.org/
endef

define Package/gnupg/description
 GnuPG is GNU's tool for secure communication and data storage.
 It can be used to encrypt data and to create digital signatures.
 It includes an advanced key management facility and is compliant
 with the proposed OpenPGP Internet standard as described in RFC2440.
 .
 GnuPG does not use any patented algorithms so it cannot be compatible
 with PGP2 because it uses IDEA (which is patented worldwide).
 .
 This version of GnuPG has basic functionality like keyserver support
 enabled, so it is larger and carries some additional dependencies,
 however this package still doesn't support every feature available.
endef

CONFIGURE_ARGS += \
	--disable-rpath \
	--disable-gnupg-iconv \
	--disable-card-support \

MAKE_FLAGS += \
	SUBDIRS="m4 intl zlib util mpi cipher tools g10 keyserver ${checks}" \

define Package/gnupg/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib/gnupg
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/gpg $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/gnupg/gpgkeys_curl $(1)/usr/lib/gnupg/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/gnupg/gpgkeys_finger $(1)/usr/lib/gnupg/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/gnupg/gpgkeys_hkp $(1)/usr/lib/gnupg/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/gnupg/gpgkeys_ldap $(1)/usr/lib/gnupg/
endef

$(eval $(call BuildPackage,gnupg))
