# SPDX-License-Identifier: GPL-2.0-or-later

include $(TOPDIR)/rules.mk

PKG_NAME:=rtl88x2bu
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE_URL:=https://github.com/RinCat/RTL88x2BU-Linux-Driver
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=15c214d798d7dbe5180139cfd921b1a8da591450
PKG_MAINTAINER:=Dirk Neukirchen <plntyk.lede@plntyk.name>
PKG_BUILD_PARALLEL:=1

#STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl88x2bu
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek 88x2BU driver
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
	  $(PKG_BUILD_DIR)/88x2bu.ko
  AUTOLOAD:=$(call AutoProbe,rtl88x2bu)
  PROVIDES:=kmod-rtl88x2bu
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(TOOLCHAIN_DIR)/include \
	-I$(STAGING_DIR)/include \
	-I$(STAGING_DIR)/usr/include

# from Makefile: obj-(CONFIG_RTL8822BU) = ...
ifdef CONFIG_PACKAGE_kmod-rtl88x2bu
   PKG_MAKE_FLAGS += CONFIG_RTL8822BU=m
endif

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT


define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		$(PKG_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		modules
endef

$(eval $(call KernelPackage,rtl88x2bu))
