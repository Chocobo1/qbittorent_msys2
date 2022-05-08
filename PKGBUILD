# Maintainer: Chocobo1 <https://github.com/Chocobo1>

_realname=qbittorrent
pkgbase=mingw-w64-${_realname}-git
pkgname=${MINGW_PACKAGE_PREFIX}-${_realname}-git
pkgver=r11712.648451a01
pkgrel=1
pkgdesc="An advanced BitTorrent client programmed in C++, based on Qt toolkit and libtorrent-rasterbar (mingw-w64)"
arch=('any')
mingw_arch=('clang32' 'clang64' 'mingw32' 'mingw64' 'ucrt64')
url="https://qbittorrent.org/"
license=('custom' 'GPL')
depends=("${MINGW_PACKAGE_PREFIX}-boost"
         "${MINGW_PACKAGE_PREFIX}-libtorrent-rasterbar"
         "${MINGW_PACKAGE_PREFIX}-qt6-base"
         "${MINGW_PACKAGE_PREFIX}-qt6-svg"
         "${MINGW_PACKAGE_PREFIX}-zlib")
makedepends=("git"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-qt6-tools")
optdepends=("${MINGW_PACKAGE_PREFIX}-python: needed for torrent search tab")
provides=("${MINGW_PACKAGE_PREFIX}-${_realname}")
conflicts=("${MINGW_PACKAGE_PREFIX}-${_realname}")
source=("git+https://github.com/qbittorrent/qBittorrent.git")
sha256sums=('SKIP')


prepare() {
  cd "$srcdir/${_realname}"

  # prepare env for msys2-mingw
  sed \
    -i \
    -e 's/NTDDI_VERSION/#NTDDI_VERSION/g' \
    -e 's/_WIN32_WINNT/#_WIN32_WINNT/g' \
    -e 's/_WIN32_IE/#_WIN32_IE/g' \
    "cmake/Modules/MacroQbtCommonConfig.cmake"
}

pkgver() {
  cd "$srcdir/${_realname}"

  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$srcdir/${_realname}"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    "${MINGW_PREFIX}/bin/cmake.exe" \
      -B "_build" \
      -DCMAKE_BUILD_TYPE=RelWithDebInfo \
      -DCMAKE_INSTALL_PREFIX="${MINGW_PREFIX}" \
      -DQT6=ON \
      ./
  "${MINGW_PREFIX}/bin/cmake.exe" \
    --build "_build"
}

package() {
  cd "$srcdir/${_realname}"

  DESTDIR="$pkgdir" \
    "${MINGW_PREFIX}/bin/cmake.exe" \
      --install "_build"
  install -Dm644 "COPYING" -t "$pkgdir/${MINGW_PREFIX}/share/licenses/${_realname}"

  rm "$pkgdir/${MINGW_PREFIX}/bin/qt.conf"
}
