# realm-openwrt
OpenWrt feed for the Realm C++ SDK

Add the following to your `feeds.conf` or `feeds.conf.default`:
```
src-git realm https://github.com/fealebenpae/realm-openwrt.git
```

and within your buildroot run:
```sh
./scripts/feeds update realm
./scripts/feeds install -a -p realm
```


Now it's possible to use `make menuconfig` to select the `Libraries/database/libcpprealm` package, or use the `realm-helloworld` package as a template for your own application.
