# $Id$
# Maintainer: Soup <soup at soultrap dot net>
# Contributor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-ll
pkgver=295.59
_extramodules=extramodules-3.4-ARCH
pkgrel=2
pkgdesc="NVIDIA drivers for linux (long-lived branch)"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux>=3.4' 'linux<3.5' "nvidia-utils-ll=${pkgver}")
makedepends=('linux-headers>=3.4' 'linux-headers<3.5')
conflicts=('nvidia-96xx' 'nvidia-173xx' 'nvidia')
license=('custom')
install=nvidia.install
options=(!strip)
provides=('nvidia')

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('bef732dfcf5cb079c06c1e8672d8d5dd')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
   _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('864d5dd1a29cb303bd355707413e2b98')
fi

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
