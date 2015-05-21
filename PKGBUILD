# Maintainer: Sven Fischer <sven (at) leiderfischer.de>
# Contributor: Thijs Vermeir <thijsvermeir@gmail.com>

pkgbase=('bcusdk')
pkgname=('eibd')
pkgver=0.0.5
pkgrel=6
pkgdesc="A free development environment for BCU1 and BCU2"
arch=('i686' 'x86_64' 'arm' 'armv7h')
url="http://www.auto.tuwien.ac.at/~mkoegler/index.php/bcusdk"
license=('GPL')
depends=('pthsem>=2.0.8' 'gcc-libs-multilib')
options=('!libtool')
source=(http://www.auto.tuwien.ac.at/~mkoegler/eib/bcusdk_${pkgver}.tar.gz
    eibd.patch eibd.socket eibd.service eibd.conf)
md5sums=('5f81bc4e6bb53564573d573e795a9a5f'
         'b9e50d68138fb74a4d0f6370a720d8fe'
         '82e079c823fa226146ba49b47c76bab7'
         '9919fe034fb3f3b1ad8c45a9daef0dde'
         '815323eebff8bc442c1e653a34e6b0b4')

prepare() {
    cd "${srcdir}/${pkgbase}-${pkgver}"
    patch -p1 -i ${srcdir}/eibd.patch
}

build() {
    cd "${srcdir}/${pkgbase}-${pkgver}"

    ./configure \
        --prefix=/usr \
        --enable-onlyeibd \
        --enable-usb \
        --enable-ft12 \
        --enable-eibnetip \
        --enable-eibnetipserver \
        --enable-eibnetiptunnel

    cd "${srcdir}/${pkgbase}-${pkgver}/common"
    make

    cd "${srcdir}/${pkgbase}-${pkgver}/eibd"
    make
}

package_eibd() {
    pkgdesc="Daemon as interface to the EIB / KNX bus"

    install -Dm644 "${srcdir}/eibd.socket" $pkgdir/usr/lib/systemd/system/eibd.socket
    install -Dm644 "${srcdir}/eibd.service" $pkgdir/usr/lib/systemd/system/eibd.service
    install -Dm644 "${srcdir}/eibd.conf" $pkgdir/etc/conf.d/eibd.conf

    cd "${srcdir}/${pkgbase}-${pkgver}/eibd"
    make DESTDIR="${pkgdir}" install

    cd "${srcdir}/${pkgbase}-${pkgver}/common"
    make DESTDIR="${pkgdir}" install

    #cd ${pkgdir}
    #rm -rf usr/share
    #cd usr/bin
    #rm -rf `ls | grep -v eib`
}
