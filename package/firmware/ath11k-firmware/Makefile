#
# Copyright (C) 2022 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ath11k-firmware
PKG_SOURCE_DATE:=2024-10-14
PKG_SOURCE_VERSION:=15f050122da5ef5bef2cc8c7c19dfb7f98060a49
PKG_MIRROR_HASH:=e7a89d1570f32552ade539376181494724b5eafb31d00451ff2dcf5a2922a0cb
PKG_RELEASE:=3

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://git.codelinaro.org/clo/ath-firmware/ath11k-firmware.git

PKG_LICENSE_FILES:=LICENSE.qca_firmware

PKG_MAINTAINER:=Robert Marko <robimarko@gmail.com>

include $(INCLUDE_DIR)/package.mk

RSTRIP:=:
STRIP:=:

ATH11K_LEGACY_FW_NAME:=ath11k-legacy-firmware
ATH11K_LEGACY_FW_DATE:=2024-03-14
ATH11K_LEGACY_FW_RELEASE:=795809c7
ATH11K_LEGACY_FW_SUBDIR:=$(ATH11K_LEGACY_FW_NAME)-$(ATH11K_LEGACY_FW_DATE)~$(ATH11K_LEGACY_FW_RELEASE)
ATH11K_LEGACY_FW_SOURCE=$(ATH11K_LEGACY_FW_SUBDIR).tar.zst
ATH11K_IPQ9574_SUBDIR:=ath11k-firmware/IPQ9574/hw1.0/2.9.0.1/WLAN.HK.2.9.0.1-01890-QCAHKSWPL_SILICONZ-1

define Download/$(ATH11K_LEGACY_FW_NAME)
	FILE:=$(ATH11K_LEGACY_FW_SOURCE)
	PROTO:=git
	URL:=https://github.com/quic/upstream-wifi-fw.git
	SOURCE_VERSION:=795809c7041582bd51bdfaa1f548b916ae8d4382
	MIRROR_HASH:=b55bf7204543c7ba87876e84788239b0e47ad62434c5b0d1169526cde36a9daa
	SUBDIR:=$(ATH11K_LEGACY_FW_SUBDIR)
endef
$(eval $(call Download,$(ATH11K_LEGACY_FW_NAME)))

# IPQ6018 WLAN.HK.2.12-01460-QCAHKSWPL_SILICONZ-1, from DD-WRT's tree
ATH11K_DDWRT_FW_NAME:=ath11k-ddwrt-firmware-ipq6018
ATH11K_DDWRT_FW_DATE:=2025-06-11
ATH11K_DDWRT_FW_RELEASE:=r61739
ATH11K_DDWRT_FW_SUBDIR:=$(ATH11K_DDWRT_FW_NAME)-$(ATH11K_DDWRT_FW_DATE)~$(ATH11K_DDWRT_FW_RELEASE)
ATH11K_DDWRT_FW_SOURCE=$(ATH11K_DDWRT_FW_SUBDIR).tar.zst

define Download/$(ATH11K_DDWRT_FW_NAME)
	FILE:=$(ATH11K_DDWRT_FW_SOURCE)
	PROTO:=svn
	URL:=svn://svn.dd-wrt.com/DD-WRT/src/router/firmwares/wireless/ath11k/IPQ6018
	SOURCE_VERSION:=61739
	MIRROR_HASH:=1eb90a54644da852dfd4d2f2229d9cc335a249c76a591fe99e005fba9e13a961
	SUBDIR:=$(ATH11K_DDWRT_FW_SUBDIR)
endef
$(eval $(call Download,$(ATH11K_DDWRT_FW_NAME)))

define Package/ath11k-firmware-default
  SECTION:=firmware
  CATEGORY:=Firmware
  URL:=$(PKG_SOURCE_URL)
  DEPENDS:=
endef

define Package/ath11k-firmware-ipq5018-qcn6122
$(Package/ath11k-firmware-default)
  TITLE:=IPQ5018/QCN6122 ath11k firmware
endef

define Package/ath11k-firmware-ipq6018
$(Package/ath11k-firmware-default)
  TITLE:=IPQ6018 ath11k firmware
endef

define Package/ath11k-firmware-ipq9574
$(Package/ath11k-firmware-default)
  TITLE:=IPQ9574 ath11k firmware
  URL:=https://github.com/quic/upstream-wifi-fw
  LICENSE:=LicenseRef-QuIC
  LICENSE_FILES:=../$(ATH11K_LEGACY_FW_SUBDIR)/ath11k-firmware/LICENSE.md
endef

define Build/Clean
	$(call Build/Clean/Default,)

	rm -rf \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR) \
		$(BUILD_DIR)/$(ATH11K_DDWRT_FW_SUBDIR)
endef

define Build/Prepare
	$(call Build/Prepare/Default,)

	$(TAR) -C $(BUILD_DIR) -xf $(DL_DIR)/$(ATH11K_LEGACY_FW_SOURCE)
	$(TAR) -C $(BUILD_DIR) -xf $(DL_DIR)/$(ATH11K_DDWRT_FW_SOURCE)
endef

define Build/Compile

endef

define Package/ath11k-firmware-ipq5018-qcn6122/install
	$(INSTALL_DIR) $(1)/lib/firmware/ath11k/IPQ5018/hw1.0
	$(INSTALL_DATA) \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/ath11k-firmware/IPQ5018_QCN6122_QCN6122/hw1.0/2.7.0.1/WLAN.HK.2.7.0.1-01744-QCAHKSWPL_SILICONZ-1/*.* \
		$(1)/lib/firmware/ath11k/IPQ5018/hw1.0/
	$(INSTALL_DIR) $(1)/lib/firmware/ath11k/QCN6122/hw1.0
	$(INSTALL_DATA) \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/ath11k-firmware/IPQ5018_QCN6122_QCN6122/hw1.0/2.7.0.1/WLAN.HK.2.7.0.1-01744-QCAHKSWPL_SILICONZ-1/qcn6122/*.* \
		$(1)/lib/firmware/ath11k/QCN6122/hw1.0/
endef

define Package/ath11k-firmware-ipq6018/install
	$(INSTALL_DIR) $(1)/lib/firmware/IPQ6018
	$(INSTALL_DATA) \
		$(BUILD_DIR)/$(ATH11K_DDWRT_FW_SUBDIR)/hw1.0/q6_fw.* \
		$(BUILD_DIR)/$(ATH11K_DDWRT_FW_SUBDIR)/hw1.0/m3_fw.* \
		$(1)/lib/firmware/IPQ6018/
endef

define Package/ath11k-firmware-ipq9574/install
	$(INSTALL_DIR) $(1)/lib/firmware/IPQ9574
	$(INSTALL_DATA) \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/$(ATH11K_IPQ9574_SUBDIR)/q6_fw.mdt \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/$(ATH11K_IPQ9574_SUBDIR)/q6_fw.b?? \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/$(ATH11K_IPQ9574_SUBDIR)/m3_fw.mdt \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/$(ATH11K_IPQ9574_SUBDIR)/m3_fw.b?? \
		$(1)/lib/firmware/IPQ9574/
	$(INSTALL_DIR) $(1)/usr/share/licenses/ath11k-firmware-ipq9574
	$(INSTALL_DATA) \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/ath11k-firmware/LICENSE.md \
		$(BUILD_DIR)/$(ATH11K_LEGACY_FW_SUBDIR)/$(ATH11K_IPQ9574_SUBDIR)/Notice.txt \
		$(1)/usr/share/licenses/ath11k-firmware-ipq9574/
endef

$(eval $(call BuildPackage,ath11k-firmware-ipq5018-qcn6122))
$(eval $(call BuildPackage,ath11k-firmware-ipq6018))
$(eval $(call BuildPackage,ath11k-firmware-ipq9574))
