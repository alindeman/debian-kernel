# alindeman's raspi debian-kernel

## Building

```bash
vagrant up
```

Within `vagrant ssh`:

```bash
rsync -avz --delete /vagrant/ debian-kernel
cd debian-kernel

(mkdir -p ../orig && cd ../orig && curl -LO 'http://deb.debian.org/debian/pool/main/l/linux/linux_5.10.13.orig.tar.xz')

ARCH=arm64
FEATURESET=none
FLAVOUR=raspi-arm64
export $(dpkg-architecture -a$ARCH)
export PATH=/usr/lib/ccache:$PATH
export DEB_BUILD_PROFILES="cross nopython nodoc pkg.linux.notools"
export MAKEFLAGS="-j$(($(nproc)*2))"
export DEBIAN_KERNEL_DISABLE_DEBUG=

fakeroot make -f debian/rules clean
fakeroot make -f debian/rules orig
fakeroot make -f debian/rules source
fakeroot make -f debian/rules.gen setup_${ARCH}_${FEATURESET}_${FLAVOUR}
fakeroot make -f debian/rules.gen binary-arch_${ARCH}_${FEATURESET}_${FLAVOUR}
```

## Customizations

### devicetree-overlay

<https://git.kernel.org/pub/scm/linux/kernel/git/geert/renesas-drivers.git/log/?h=topic/overlays>

<https://elinux.org/R-Car/DT-Overlays>
