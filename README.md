# yocto_pi_rust_app
Template for building raspberry pi image running a rust app.

## run the rust app

In the folder hello world you will find the hello world app.

```sh
cd hello-world
cargo build
cargo run
```

## bitbake

```sh
cd hello-world
cargo install cargo-bitbake --locked
cargo bitbake
```

## yocto build

Based on https://medium.com/nerd-for-tech/build-your-own-linux-image-for-the-raspberry-pi-f61adb799652

```sh
mkdir sources
cd sources
# add sources
git clone git://git.yoctoproject.org/poky -b dunfell --depth=1 
git clone git://git.yoctoproject.org/meta-raspberrypi -b dunfell --depth=1 
git clone git://git.openembedded.org/meta-openembedded -b dunfell --depth=1
git clone https://github.com/meta-rust/meta-rust.git
# load env for raspberry
cd .. 
source sources/poky/oe-init-build-env
```

### update build/conf/bblayers.conf

```conf
# POKY_BBLAYERS_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
POKY_BBLAYERS_CONF_VERSION = "2"

BBPATH = "${TOPDIR}"
BBFILES ?= "../hello-world/hello-world_0.1.0.bb"

BBLAYERS ?= " \
  ${TOPDIR}/../sources/poky/meta \
  ${TOPDIR}/../sources/poky/meta-poky \
  ${TOPDIR}/../sources/poky/meta-yocto-bsp \
  ${TOPDIR}/../sources/meta-raspberrypi \
  ${TOPDIR}/../sources/meta-openembedded/meta-oe \
  ${TOPDIR}/../sources/meta-rust \
  "
```

### build image

```sh
MACHINE=raspberrypi3 bitbake core-image-minimal
```
