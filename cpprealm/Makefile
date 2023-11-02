include $(TOPDIR)/rules.mk

PKG_NAME:=cpprealm
PKG_VERSION:=0.4.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/realm/realm-cpp.git
PKG_SOURCE_VERSION:=yg/openwrt-updates

PKG_CONFIG_DEPENDS := \
	CONFIG_REALM_ENCRYPTION \
	CONFIG_REALM_SYNC \
	CONFIG_REALM_GEOSPATIAL \
	CONFIG_REALM_SHARED_LIBS \
	CONFIG_REALM_LTO

PKG_BUILD_DEPENDS:=(CONFIG_REALM_SYNC||CONFIG_REALM_ENCRYPTION):openssl curl zlib libuv

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libcpprealm
    TITLE:=Realm C++ SDK
	URL:=https://github.com/realm/realm-cpp
    SUBMENU:=database
	SECTION:=libs
	CATEGORY:=Libraries
	DEPENDS:=+libstdcpp +libpthread +zlib +(REALM_SYNC||REALM_ENCRYPTION):libopenssl +libcurl +libuv
endef

define Package/libcpprealm/config
  source "$(SOURCE)/Config.in"
endef

CMAKE_BINARY_SUBDIR := build
CMAKE_OPTIONS += -DREALM_USE_SYSTEM_OPENSSL=ON -DOPENSSL_USE_STATIC_LIBS=OFF -DREALM_CPP_NO_TESTS=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON \
	-DREALM_ENABLE_ENCRYPTION=$(if $(CONFIG_REALM_ENCRYPTION),ON,OFF) \
	-DREALM_ENABLE_SYNC=$(if $(CONFIG_REALM_SYNC),ON,OFF) \
	-DREALM_ENABLE_GEOSPATIAL=$(if $(CONFIG_REALM_GEOSPATIAL),ON,OFF) \
	-DBUILD_SHARED_LIBS=$(if $(CONFIG_REALM_SHARED_LIBS),ON,OFF) \
	-DCMAKE_INTERPROCEDURAL_OPTIMIZATION=$(if $(CONFIG_REALM_LTO),ON,OFF)
EXTRA_CXXFLAGS:=-Wno-psabi

define Build/InstallDev-static
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.a $(1)/usr/lib/
endef

define Build/InstallDev-shared
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libcpprealm.so $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(if $(CONFIG_REALM_SHARED_LIBS),$(call Build/InstallDev-shared,$(1)),$(call Build/InstallDev-static,$(1)))
endef

define Package/libcpprealm/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(if $(CONFIG_REALM_SHARED_LIBS),$(call Build/InstallDev-shared,$(1)))
endef

$(eval $(call BuildPackage,libcpprealm))