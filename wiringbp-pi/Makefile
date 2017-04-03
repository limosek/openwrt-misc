#
# Copyright (C) 2015 OpenWrt
# See LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wiringbp-pi
PKG_RELEASE:=1

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/wiringbp-pi
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=WiringBP
  MAINTAINER:=Gerhard Bertelsmann
  DEPENDS:= +libpthread +librt
endef

define Package/wiringbp-pi/description
  This is a modified WiringPi for Banana Pi
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./* $(PKG_BUILD_DIR)/
endef

MOPTS = PREFIX=/usr/

define Build/Compile
	make -C $(PKG_BUILD_DIR)/wiringPi	\
 		$(TARGET_CONFIGURE_OPTS) $(MOPTS)	\
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I/$(PKG_BUILD_DIR)/wiringPi/ -I/$(PKG_BUILD_DIR)/devLib/"	\
		LIBS="$(TARGET_LDFLAGS)"
	ln -sf $(PKG_BUILD_DIR)/wiringPi/libwiringPi.so.2.0 $(PKG_BUILD_DIR)/wiringPi/libwiringPi.so
	make -C $(PKG_BUILD_DIR)/devLib		\
		$(TARGET_CONFIGURE_OPTS) $(MOPTS)	\
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I/$(PKG_BUILD_DIR)/wiringPi/ -I/$(PKG_BUILD_DIR)/devLib/"	\
		LIBS="$(TARGET_LDFLAGS)"
	ln -sf $(PKG_BUILD_DIR)/devLib/libwiringPiDev.so.2.0 $(PKG_BUILD_DIR)/devLib/libwiringPiDev.so
	make -C $(PKG_BUILD_DIR)/gpio		\
		$(TARGET_CONFIGURE_OPTS) $(MOPTS)	\
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I/$(PKG_BUILD_DIR)/wiringPi/ -I/$(PKG_BUILD_DIR)/devLib/"	\
		LIBS="$(TARGET_LDFLAGS) -L$(PKG_BUILD_DIR)/devLib -L$(PKG_BUILD_DIR)/wiringPi -lwiringPi -lwiringPiDev -lpthread -lm"
	make -C $(PKG_BUILD_DIR)/examples speed	\
		$(TARGET_CONFIGURE_OPTS)  $(MOPTS)	\
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I/$(PKG_BUILD_DIR)/wiringPi/ -I/$(PKG_BUILD_DIR)/devLib/"	\
		LIBS="$(TARGET_LDFLAGS) -L$(PKG_BUILD_DIR)/devLib -L$(PKG_BUILD_DIR)/wiringPi -lwiringPi -lwiringPiDev -lm"
endef

define Build/InstallDev
        make -C $(PKG_BUILD_DIR)/wiringPi $(MOPTS) DESTDIR=$(1) \
        	install-headers install-static
endef
        
define Package/wiringbp-pi/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wiringPi/libwiringPi.so.2.0 $(1)/usr/lib
	ln -sf libwiringPi.so.2.0     $(1)/usr/lib/libwiringPi.so
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/devLib/libwiringPiDev.so.2.0 $(1)/usr/lib
	ln -sf libwiringPiDev.so.2.0  $(1)/usr/lib/libwiringPiDev.so
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/gpio/gpio $(1)/usr/bin/gpio
endef

$(eval $(call BuildPackage,wiringbp-pi))
