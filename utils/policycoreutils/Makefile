#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=policycoreutils
PKG_VERSION:=3.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/SELinuxProject/selinux/releases/download/20200710
PKG_HASH:=c889f62ee80f8b6a369469a9b8af51f5b797975aeaa291f5c5960cc12eed1934
PKG_INSTALL:=1
HOST_BUILD_DEPENDS:=libsemanage/host

PKG_MAINTAINER:=Thomas Petazzoni <thomas.petazzoni@bootlin.com>

include $(INCLUDE_DIR)/package.mk

define Package/policycoreutils
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:= +libsemanage +libcap-ng
  TITLE:=SELinux policy utilities
  URL:=http://selinuxproject.org/page/Main_Page
endef

define Package/policycoreutils/description
	Policycoreutils is a collection of policy utilities
	(originally the "core" set of utilities needed to use
	SELinux, although it has grown a bit over time), which have
	different dependencies.  sestatus, secon, run_init, and
	newrole only use libselinux. load_policy and setfiles only
	use libselinux and libsepol. semodule and semanage use
	libsemanage (and thus bring in dependencies on libsepol and
	libselinux as well). setsebool uses libselinux to make
	non-persistent boolean changes (via the kernel interface)
	and uses libsemanage to make persistent boolean changes.
endef

include $(INCLUDE_DIR)/host-build.mk

HOST_MAKE_FLAGS += \
	PREFIX=$(STAGING_DIR_HOSTPKG) \
	SBINDIR=$(STAGING_DIR_HOSTPKG)/sbin \
	ETCDIR=$(STAGING_DIR_HOSTPKG)/etc

define Package/policycoreutils/install
	# Remove programs provided by BusyBox.
	$(RM) -f $(PKG_INSTALL_DIR)/sbin/restorecon
	$(RM) -f $(PKG_INSTALL_DIR)/sbin/setfiles
	$(RM) -f $(PKG_INSTALL_DIR)/usr/sbin/load_policy
	$(RM) -f $(PKG_INSTALL_DIR)/usr/sbin/sestatus
	$(RM) -f $(PKG_INSTALL_DIR)/usr/sbin/setsebool
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/* $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/sbin
	$(CP) $(PKG_INSTALL_DIR)/sbin/* $(1)/sbin/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,policycoreutils))
