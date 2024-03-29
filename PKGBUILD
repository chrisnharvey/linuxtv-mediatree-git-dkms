# Maintainer: Chris Harveu <chris@chrisnharvey.com>
# Based on the work in tbs-linux_media-git-dkms

_pkgbase=tbs-linux_media-git-dkms
pkgname=linuxtv-mediatree-git-dkms
pkgver=r20180926.102742.6023ef2c0
pkgrel=1
pkgdesc="LinuxTV mediatree modules (DKMS)"
arch=('i686' 'x86_64')
url="https://www.linuxtv.org/wiki/index.php/How_to_Obtain,_Build_and_Install_V4L-DVB_Device_Drivers"
license=('GPL2')
makedepends=('git')
depends=('dkms' 'patchutils' 'perl-proc-processtable')
conflicts=("${_pkgbase}" 'tbs-dvb-drivers')
provides=("${_pkgbase}" 'linux_media')
source=('dkms.conf'
        'modules.list'
        'media_build::git://linuxtv.org/media_build.git')
sha256sums=('0fa837177caccf36c419206451f880439e38bb4f8e82a087e1a70f5d05ebda19'
            '195c6a971c915855ab4e39cfd4d7ae14b513fbb7c8daa5b3e5135cc5b50ba81c'
            'SKIP')
options=('!strip')

prepare() {
    if [ -d "${srcdir}/media" ]; then
        cd "${srcdir}/media" && \
        git fetch --depth=1 origin && \
        git reset --hard origin/master
    else
        git clone --depth=1 git://linuxtv.org/media_tree.git -b master "$srcdir/media"
    fi
}

pkgver() {
    cd "$srcdir/media"
    printf "r%s.%s" "$(git show -s --date=format:'%Y%m%d.%H%M%S' --format=%cd)" "$(git rev-parse --short HEAD)"
}

package() {
    # Copy dkms.conf
    install -Dm644 "${srcdir}/dkms.conf" "${pkgdir}/usr/src/${pkgname}-${pkgver}/dkms.conf"

    # Set name and version
    sed -e "s/@_PKGNAME@/${pkgname}/" \
        -e "s/@PKGVER@/${pkgver}/" \
        -i "${pkgdir}/usr/src/${pkgname}-${pkgver}/dkms.conf"

    # Append module list
    cat "${srcdir}/modules.list" >> "${pkgdir}/usr/src/${pkgname}-${pkgver}/dkms.conf"

    # Copy sources
    mkdir -p "${pkgdir}/usr/src/${pkgname}-${pkgver}/media/"
    cp -r "${srcdir}/media"/* "${pkgdir}/usr/src/${pkgname}-${pkgver}/media/"

    mkdir -p "${pkgdir}/usr/src/${pkgname}-${pkgver}/media_build/"
    cp -r "${srcdir}/media_build"/* "${pkgdir}/usr/src/${pkgname}-${pkgver}/media_build/"
}
