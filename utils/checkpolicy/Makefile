#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=checkpolicy
PKG_VERSION:=2.9
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/SELinuxProject/selinux/releases/download/20190315
PKG_HASH:=a946c32b284532447857e4c48830f8816867c61220c8c08bdd32e6f691335f8e
HOST_BUILD_DEPENDS:=libselinux/host

PKG_MAINTAINER:=Thomas Petazzoni <thomas.petazzoni@bootlin.com>

include $(INCLUDE_DIR)/package.mk

define Package/checkpolicy
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=SELinux policy compiler
  URL:=http://selinuxproject.org/page/Main_Page
endef

define Package/checkpolicy/description
	checkpolicy is the SELinux policy compiler. It uses libsepol
	to generate the binary policy. checkpolicy uses the static
	libsepol since it deals with low level details of the policy
	that have not been encapsulated/abstracted by a proper
	shared library interface.
endef

include $(INCLUDE_DIR)/host-build.mk

HOST_MAKE_FLAGS += \
	PREFIX=$(STAGING_DIR_HOSTPKG)

$(eval $(call HostBuild))
$(eval $(call BuildPackage,checkpolicy))
