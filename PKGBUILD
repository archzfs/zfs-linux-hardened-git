# Maintainer: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-hardened packages will only work with the default linux-hardened package! To have a single PKGBUILD target
# many kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
#
pkgbase="zfs-linux-hardened-git"
pkgname=("zfs-linux-hardened-git" "zfs-linux-hardened-git-headers")

pkgver=0.7.0.r194.g94183a9d8.4.13.14.a.1
pkgrel=1
makedepends=("linux-hardened-headers=4.13.14.a-1" "git" "spl-linux-hardened-git-headers")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("git+https://github.com/zfsonlinux/zfs.git")
sha256sums=("SKIP")
license=("CDDL")
depends=("kmod" "spl-linux-hardened-git" "zfs-utils-common-git>=0.7.0.r194.g94183a9d8" "linux-hardened=4.13.14.a-1")

pkgver() {
    cd "${srcdir}/zfs"
    echo $(git describe --long | sed 's/^zfs-//;s/\([^-]*-g\)/r\1/;s/-/./g').4.13.14.a.1
}

build() {
    cd "${srcdir}/zfs"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/lib/udev \
                --libexecdir=/usr/lib/zfs-0.7.3 --with-config=kernel \
                --with-linux=/usr/lib/modules/4.13.14-1-hardened/build \
                --with-linux-obj=/usr/lib/modules/4.13.14-1-hardened/build
    make
}

package_zfs-linux-hardened-git() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs")
    groups=("archzfs-linux-hardened-git")
    conflicts=('zfs-linux-hardened')
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-hardened-git-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    conflicts=('zfs-archiso-linux-headers' 'zfs-linux-hardened-headers'  'zfs-linux-lts-headers' 'zfs-linux-lts-git-headers' 'zfs-linux-headers' 'zfs-linux-git-headers' 'zfs-linux-zen-headers' 'zfs-linux-zen-git-headers' )
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/4.13.14-1-hardened/Module.symvers
}
