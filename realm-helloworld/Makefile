include $(TOPDIR)/rules.mk

PKG_NAME:=realm-helloworld
PKG_VERSION:=1.0
PKG_RELEASE:=1

PKG_BUILD_DEPENDS:=cpprealm

include $(INCLUDE_DIR)/package.mk

define Package/realm-helloworld
  SECTION:=examples
  CATEGORY:=Examples
  TITLE:=Hello, Realm!
  DEPENDS:=+libcpprealm
endef

define Package/realm-helloworld/description
  A simple "Hello, world!" -application.
endef

define Build/Prepare
		mkdir -p $(PKG_BUILD_DIR)
endef

TARGET_CXXFLAGS+=-std=c++17 -fexceptions -frtti -Wno-psabi
LIBRARY_DEPENDENCIES:=-lcpprealm -lrealm-object-store -lrealm-sync -lrealm-parser -lrealm -lcurl -lssl -lcrypto -luv -lz -lstdc++fs

define Build/Compile
		$(TARGET_CXX) $(TARGET_CPPFLAGS) $(TARGET_CXXFLAGS) -o $(PKG_BUILD_DIR)/realm-helloworld.o -c realm-helloworld.cpp
		$(TARGET_CXX) $(TARGET_LDFLAGS) -o $(PKG_BUILD_DIR)/realm-helloworld $(PKG_BUILD_DIR)/realm-helloworld.o $(LIBRARY_DEPENDENCIES)
endef

define Package/realm-helloworld/install
		$(INSTALL_DIR) $(1)/usr/bin
		$(INSTALL_BIN) $(PKG_BUILD_DIR)/realm-helloworld $(1)/usr/bin
endef

$(eval $(call BuildPackage,realm-helloworld))