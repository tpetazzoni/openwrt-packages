#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

SRC_NAME:=libselinux
PKG_NAME:=python-$(SRC_NAME)
PKG_VERSION:=3.1
PKG_RELEASE:=1

PKG_SOURCE:=$(SRC_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/SELinuxProject/selinux/releases/download/20200710
PKG_HASH:=ea5dcbb4d859e3f999c26a13c630da2f16dff9462e3cc8cb7b458ac157d112e7
PKG_BUILD_DEPENDS:=PACKAGE_python-libselinux:python3 PACKAGE_python-libselinux:swig/host
HOST_BUILD_DEPENDS:=libsepol/host pcre/host

PKG_MAINTAINER:=Thomas Petazzoni <thomas.petazzoni@bootlin.com>

include $(INCLUDE_DIR)/package.mk
include ../../lang/python/python3-package.mk

define Package/python-libselinux
  SUBMENU:=Python
  SECTION:=lang
  DEPENDS:=+libsepol +libpcre +musl-fts +@KERNEL_SECURITY +@KERNEL_SECURITY_NETWORK +@KERNEL_SECURITY_SELINUX +python3 +libselinux
  CATEGORY:=Libraries
  TITLE:=Python bindings for the runtime SELinux library
  URL:=http://selinuxproject.org/page/Main_Page
  CATEGORY:=Languages
endef

define Package/python-libselinux/description
	libselinux is the runtime SELinux library that provides
	interfaces (e.g. library functions for the SELinux kernel
	APIs like getcon(), other support functions like
	getseuserbyname()) to SELinux-aware applications. libselinux
	may use the shared libsepol to manipulate the binary policy
	if necessary (e.g. to downgrade the policy format to an
	older version supported by the kernel) when loading policy.

	This package provides the Python bindings for libselinux.
endef

include $(INCLUDE_DIR)/host-build.mk

# Needed to link libselinux utilities, which link against
# libselinux.so, which indirectly depends on libpcre.so, installed in
# $(STAGING_DIR_HOSTPKG).
HOST_LDFLAGS += -Wl,-rpath="$(STAGING_DIR_HOSTPKG)/lib"

HOST_MAKE_FLAGS += \
	PREFIX=$(STAGING_DIR_HOSTPKG) \
	SHLIBDIR=$(STAGING_DIR_HOSTPKG)/lib

$(eval $(call HostBuild))

MAKE_FLAGS += \
	FTS_LDLIBS=-lfts \
	SHLIBDIR=/usr/lib \
	PYTHON=$(PYTHON3) \
	PYINC="-I $(PYTHON3_INC_DIR)" \
	PYLIBS="-lpython$(PYTHON3_VERSION)" \

define Build/Compile/python-libselinux
	$(call Build/Compile/Default,swigify pywrap)
endef

define Build/Install/python-libselinux
	$(call Build/Install/Default,install-pywrap)
endef

define Build/Compile
	$(call Build/Compile/Default,all)
	$(Build/Compile/python-libselinux)
endef

define Build/Install
	$(call Build/Install/Default,install)
	$(Build/Install/python-libselinux)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libselinux.pc $(1)/usr/lib/pkgconfig/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libselinux))
$(eval $(call Py3Package,python-libselinux))
$(eval $(call BuildPackage,python-libselinux))
