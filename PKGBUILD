# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
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
pkgbase="zfs-linux-hardened-git"
pkgname=("zfs-linux-hardened-git" "zfs-linux-hardened-git-headers")
_commit='01c4f2bf2933319e876e8f695e97d0719a9db0ed'
_zfsver="2020.04.08.r5842.g01c4f2bf2"
_kernelver="5.6.3.a-1"
_extramodules="5.6.3.a-1-hardened"

pkgver="${_zfsver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-hardened-headers=${_kernelver}" "git")
arch=("x86_64")
url="https://zfsonlinux.org/"
source=("git+https://github.com/zfsonlinux/zfs.git#commit=${_commit}"
        "linux-5.5-compat-blkg_tryget.patch"
        "linux-5.6-compat-struct-proc_ops.patch"
        "linux-5.6-compat-timestamp_truncate.patch"
        "linux-5.6-compat-ktime_get_raw_ts64.patch"
        "linux-5.6-compat-time_t.patch")
sha256sums=("SKIP"
            "daae58460243c45c2c7505b1d88dcb299ea7d92bcf3f41d2d30bc213000bb1da"
            "05ca889a89b1e57d55c1b7d4d3013398a3e5a69d0fad27278aad701f0bb6e802"
            "5ad4393b334a8f685212f47b44e98dc468c70214ee5dbbab24cc95c4f310ae39"
            "7c6ebee72d864160b376fc18017c81f499f177b7d9265f565de859139805a277"
            "06f7ade5adcbfe77cb234361f8b2aca6d6e78fcd136da6d3a70048b5e92c62bb")
license=("CDDL")
depends=("kmod" "zfs-utils-git=${_zfsver}" "linux-hardened=${_kernelver}")

build() {
    cd "${srcdir}/zfs"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/lib/udev \
                --libexecdir=/usr/lib/zfs-${_zfsver} --with-config=kernel \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build
    make
}

package_zfs-linux-hardened-git() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs" "spl")
    groups=("archzfs-linux-hardened-git")
    conflicts=("zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" 'zfs-linux-hardened' 'spl-linux-hardened-git' 'spl-linux-hardened')
    replaces=("spl-linux-hardened-git")
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-hardened-git-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    provides=("zfs-headers" "spl-headers")
    conflicts=("zfs-headers" "zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" "spl-headers")
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/${_extramodules}/Module.symvers
}
