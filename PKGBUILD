# Maintainer: Chocobo1 <https://github.com/Chocobo1>

_realname=qbittorrent
pkgbase=mingw-w64-${_realname}-git
pkgname=${MINGW_PACKAGE_PREFIX}-${_realname}-git
pkgver=r10437.e86cef449
pkgrel=1
pkgdesc="An advanced BitTorrent client programmed in C++, based on Qt toolkit and libtorrent-rasterbar (mingw-w64)"
arch=('any')
url="https://qbittorrent.org/"
license=('custom' 'GPL')
depends=("${MINGW_PACKAGE_PREFIX}-boost"
         "${MINGW_PACKAGE_PREFIX}-qt5"
         "${MINGW_PACKAGE_PREFIX}-libtorrent-rasterbar"
         "${MINGW_PACKAGE_PREFIX}-zlib")
makedepends=("git"
             "${MINGW_PACKAGE_PREFIX}-pkg-config")
optdepends=("${MINGW_PACKAGE_PREFIX}-python: needed for torrent search tab")
provides=("${MINGW_PACKAGE_PREFIX}-${_realname}")
conflicts=("${MINGW_PACKAGE_PREFIX}-${_realname}")
source=("git+https://github.com/qbittorrent/qBittorrent.git")
sha256sums=('SKIP')


prepare() {
  cd "$srcdir/${_realname}"

  # prepare env for msys2-mingw
  sed -i 's/!haiku/#!haiku/g' "unixconf.pri"
  sed -i 's/NTDDI_VERSION=0x06010000/NTDDI_VERSION=0x06020000/g' "winconf.pri"
  sed -i 's/_WIN32_WINNT=0x0601/_WIN32_WINNT=0x0602/g' "winconf.pri"
  sed -i 's/_WIN32_IE=0x0601/_WIN32_IE=0x0602/g' "winconf.pri"
  sed -i 's/unix:!macx:/unix|win32-g++:/g' "src/src.pro"

  # https://github.com/msys2/MINGW-packages/blob/master/mingw-w64-qbittorrent/002-fix-iconv-linking.patch
  echo "LIBS -= -lIconv::Iconv" >> "conf.pri.in"
  echo "LIBS += -lIconv" >> "conf.pri.in"
}

pkgver() {
  cd "$srcdir/${_realname}"

  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$srcdir/${_realname}"

  ./bootstrap.sh
  ./configure \
    --prefix=${MINGW_PREFIX} \
    --build=${MINGW_CHOST} \
    --host=${MINGW_CHOST} \
    --target=${MINGW_CHOST} \
    --with-boost-system=boost_system-mt
  make
}

package() {
  cd "$srcdir/${_realname}"

  make INSTALL_ROOT=$pkgdir install
  install -Dm644 "COPYING" -t "$pkgdir${MINGW_PREFIX}/share/licenses/${_realname}"
}
