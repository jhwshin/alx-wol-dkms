# Maintainer: jhwshin
# Contributor: Christoph Heiss <contact(at)christoph-heiss(dot)at>
# Contributor: Edward Pacman <edward(at)edward(minus)p(dot)xyz>
# Contributor: Kurobac <kurobac(at)foxmail(dot)com>
# Contributor: angelsl

_pkgbase=alx-wol
_kernel_tag=6.11.8-arch1
pkgname=${_pkgbase}-dkms
pkgver=6
pkgrel=3
pkgdesc="The alx kernel module with a patch enabling WoL applied"
url="https://bugzilla.kernel.org/show_bug.cgi?id=61651"
license=("GPL")
arch=('i686' 'x86_64')
depends=('dkms')
optdepends=('linux-headers: Needed for build the module for Arch kernel'
	'linux-lts-headers: Needed for build the module for LTS Arch kernel'
	'linux-zen-headers: Needed for build the module for ZEN Arch kernel')
source=("alx.h::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/alx.h"
        "ethtool.c::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/ethtool.c"
        "hw.c::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/hw.c"
        "hw.h::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/hw.h"
        "main.c::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/main.c"
        "reg.h::https://raw.githubusercontent.com/archlinux/linux/v${_kernel_tag}/drivers/net/ethernet/atheros/alx/reg.h"
        alx-wol.patch
        Kbuild
        dkms.conf)
sha256sums=('0ac6445e832c3413be3887917203699139ec05553270c5006b5a33ba5e2a158d'
            '81a568f17c736bcc8dbd0db1775e7d4877c2f063f74ee11754d320664b4ecb00'
            'bf9ac0fa98031523e05d5d62c08ee116fd4a437f66538c95eac18adda10ff89b'
            'fcc0306b37382cf8e2f75eafb6bd8696a4a5e360110e5cfe800485f4eebe55aa'
            'a1c53a58a68aa9f3b5e56fc2cae2febab695af5707547154c822601fca895851'
            '88a23ab8e6fe814efe81910bd8806f5988d367a628458154edaebdf4ccbe4902'
            'f4f10d208c81ebf6e130b632bd936426011b3e4e36a3a4dc4a6587a5007ab2e7'
            '8a62bb5adf6e554f470f88f8da58da0ce05cb8276ab56fb45ed5d31c71665d39'
            'f3aefc85fd246a74ee2be831c15b6440d22c497e84fba1f57e152c82f2946d1a')

prepare(){
    mkdir "${srcdir}/workdir"
    cd "${srcdir}/workdir"
    cp "${srcdir}/"{alx.h,ethtool.c,hw.c,hw.h,main.c,reg.h} .
    patch -p6 < ../alx-wol.patch
}

package() {
    local dest="$pkgdir/usr/src/${_pkgbase}-$pkgver"
    mkdir -p $dest
    cd "${srcdir}/workdir/"
    cp --preserve=all {alx.h,ethtool.c,hw.c,hw.h,main.c,reg.h} $dest
    cd "${srcdir}"
    cp --preserve=all Kbuild dkms.conf $dest
}
