#
# Copyright (C) 2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rtw88-usb
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2024-11-01
PKG_SOURCE_URL:=https://github.com/lwfinger/rtw88.git
PKG_SOURCE_VERSION:=e9a6539a81a4ac1a539f510ded6a91c42efb7e2e
PKG_MIRROR_HASH:=a1d667d3bbee5a9ac197a807950cdc0f6a60828898e42ca0cfb402cf6089eb0c

PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtw88-default
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek rtw88 family usb driver
  DEPENDS:=@(LINUX_6_1||LINUX_6_6) +kmod-mac80211 \
	+@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
endef

define KernelPackage/rtw88usb
  $(KernelPackage/rtw88-default)
  HIDDEN:=1
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_core.ko \
	$(PKG_BUILD_DIR)/rtw_usb.ko
  DEPENDS+=+kmod-usb-core
endef

define KernelPackage/rtl8723du
  $(KernelPackage/rtw88-default)
  TITLE:=Realtek RTL8723DU support
  DEPENDS+=+kmod-rtw88usb +rtl8723de-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_8723x.ko \
	$(PKG_BUILD_DIR)/rtw_8723d.ko \
	$(PKG_BUILD_DIR)/rtw_8723du.ko
  AUTOLOAD:=$(call AutoProbe,rtw_8723du)
endef

define KernelPackage/rtl8821au
  $(KernelPackage/rtw88-default)
  TITLE:=Realtek RTL8812AU/RTL8821AU support
  DEPENDS+=+kmod-rtw88usb +rtl8821au-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_88xxa.ko \
	$(PKG_BUILD_DIR)/rtw_8812a.ko \
	$(PKG_BUILD_DIR)/rtw_8821a.ko \
	$(PKG_BUILD_DIR)/rtw_8812au.ko \
	$(PKG_BUILD_DIR)/rtw_8821au.ko
  AUTOLOAD:=$(call AutoProbe,rtw_8812au rtw_8821au)
endef

define KernelPackage/rtl8821cu
  $(KernelPackage/rtw88-default)
  TITLE:=Realtek RTL8821CU support
  DEPENDS+=+kmod-rtw88usb +rtl8821ce-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_8821c.ko \
	$(PKG_BUILD_DIR)/rtw_8821cu.ko
  AUTOLOAD:=$(call AutoProbe,rtw_8821cu)
endef

define KernelPackage/rtl8822bu
  $(KernelPackage/rtw88-default)
  TITLE:=Realtek RTL8822BU support
  DEPENDS+=+kmod-rtw88usb +rtl8822be-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_8822b.ko \
	$(PKG_BUILD_DIR)/rtw_8822bu.ko
  AUTOLOAD:=$(call AutoProbe,rtw_8822bu)
endef

define KernelPackage/rtl8822cu
  $(KernelPackage/rtw88-default)
  TITLE:=Realtek RTL8822CU support
  DEPENDS+=+kmod-rtw88usb +rtl8822ce-firmware
  FILES:= \
	$(PKG_BUILD_DIR)/rtw_8822c.ko \
	$(PKG_BUILD_DIR)/rtw_8822cu.ko
  AUTOLOAD:=$(call AutoProbe,rtw_8822cu)
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

NOSTDINC_FLAGS+=-DBUILD_OPENWRT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

define KernelPackage/rtl8821au/install
	$(INSTALL_DIR) $(1)/lib/firmware/rtw88
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/firmware/rtw8812a_fw.bin $(1)/lib/firmware/rtw88
endef

$(eval $(call KernelPackage,rtw88usb))
$(eval $(call KernelPackage,rtl8723du))
$(eval $(call KernelPackage,rtl8821au))
$(eval $(call KernelPackage,rtl8821cu))
$(eval $(call KernelPackage,rtl8822bu))
$(eval $(call KernelPackage,rtl8822cu))
