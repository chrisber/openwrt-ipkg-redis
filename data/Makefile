include $(TOPDIR)/rules.mk

PKG_NAME:=redis
PKG_VERSION:=3.2.8
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://download.redis.io/releases/
#PKG_MD5SUM:=ea92053cbb6f4eb8a4347dbaac7d7dff

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

MAKE_FLAGS+= \
	MALLOC="libc" \
	PREFIX="$(PKG_INSTALL_DIR)/usr"

define Package/redis
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Advanced key-value cache and store
  DEPENDS:=+libpthread
  URL:=http://redis.io
endef

define Package/redis/description
	Redis is an open source, BSD licensed, advanced key-value cache and store.
	It is often referred to as a data structure server since keys can contain
	strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.
endef

# define Package/redis/conffiles
# /etc/redis.conf
# endef

MAKE_VARS = \
        CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS) $(TARGET_CPPFLAGS) $(EXTRA_CPPFLAGS)" \
        CXXFLAGS="$(TARGET_CXXFLAGS) $(EXTRA_CXXFLAGS) $(TARGET_CPPFLAGS) $(EXTRA_CPPFLAGS)" \
        LDFLAGS="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS)"


MAKE_FLAGS = \
        $(TARGET_CONFIGURE_OPTS) \
        ARCH=""



define Build/Compile

	$(info "******************************" )
	$(info $(TARGET_CC_NOCACHE) )
	$(info $(TARGET_CXX_NOCACHE) )
	$(info $(CONFIGURE_VARS) )
	$(info "******************************" )
	


	# $(MAKE) -C "$(PKG_BUILD_DIR)/deps/hiredis" static $(MAKE_VARS)  $(MAKE_FLAGS) 
	# $(MAKE) -C "$(PKG_BUILD_DIR)/deps/linenoise"  $(MAKE_VARS)  $(MAKE_FLAGS) 
	# $(MAKE) -C "$(PKG_BUILD_DIR)/deps/lua" posix $(MAKE_VARS)  $(MAKE_FLAGS)  AR="${AR} ru" 
	# $(MAKE) -C "$(PKG_BUILD_DIR)/deps/geohash-int"  $(MAKE_VARS)  $(MAKE_FLAGS) 
	sed -i 's/int err = madvise(addr, size, JEMALLOC_MADV_PURGE);//g' "$(PKG_BUILD_DIR)/deps/jemalloc/src/pages.c"
	sed -i 's/unzeroed = (!JEMALLOC_MADV_ZEROS || err != 0);//g' "$(PKG_BUILD_DIR)/deps/jemalloc/src/pages.c"
	cd "$(PKG_BUILD_DIR)/deps/jemalloc"; $(CONFIGURE_VARS)  ./configure --host=x86_64 --with-lg-quantum=3 --with-jemalloc-prefix=je_ --enable-cc-silence 
	cd "$(PKG_BUILD_DIR)/deps/jemalloc"; JEMALLOC_MADV_ZEROS="" $(MAKE_VARS) $(MAKE) $(MAKE_FLAGS)   lib/libjemalloc.a
	$(call Build/Compile/Default)
endef

define Package/redis/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME)-cli $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME)-server $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME)-check-dump $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME)-check-aof $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/$(PKG_NAME)-benchmark $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/redis.init $(1)/etc/init.d/redis
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/redis.conf $(1)/etc
endef

$(eval $(call BuildPackage,redis))


	# sed -e 's#^\(REAL_CFLAGS.* \)$$$$(ARCH)#\1#g' -i $(PKG_BUILD_DIR)/deps/hiredis/Makefile
	# sed -e 's#^\([[:space:]]*$$$$(AR)\)#\1 rcu#g' -i $(PKG_BUILD_DIR)/deps/lua/src/Makefile
	# $(call Build/Compile/Default,-C $(PKG_BUILD_DIR)/deps hiredis linenoise lua PLAT=linux)
	# $(call Build/Compile/Default)
