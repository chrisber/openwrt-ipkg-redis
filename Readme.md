## Creats a build root within a docker container

Edit `docker/env_make` to specify the location of the redis data directory
```make
Full_PATH_TO_REDIS= \
<fullpath>/openwrt-ipkg-redis/data/`
```
Build the docker container
```shell
cd docker && make build 
```


## Compile Redis manually

Uses a buildroot docker container and compiles the redis ipk manually
```shell
cd docker && make shell`
cd openwrt && make package/redis/compile V=s
```
## Compile for a different architecture

Change into the container environment
```shell
cd docker && make shell
```

Change menuconfig settings suitable for your needs
```shell
cd openwrt && make menuconfig
```
Recompile for openwrt
```shell
make -j4
```

## Copy build result from the container to the host environment

Select the container id e.g b4c2ad31325c
```shell
docker ps
```

Copy openwrt binaries form the container to the host
```shell
docker cp b4c2ad31325c:/home/openwrt/openwrt/bin/x86/packages/base/redis_3.2.8-1_x86.ipk /tmp
```


## Preserve menuconfig settings
```shell
docker cp b4c2ad31325c:/home/openwrt/openwrt/.config <Fullpath to git repo>openwrt-ipkg-redis/docker/
```

## Note UCLIB vs MUSL
MUSL does not define a __MUSL__ macro
