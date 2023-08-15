# Maintainer: Chocobo1 <https://github.com/Chocobo1>

_realname=qbittorrent
pkgbase=mingw-w64-${_realname}-git
pkgname=${MINGW_PACKAGE_PREFIX}-${_realname}-git
pkgver=4.5.4.r362.g0f862fcf9
pkgrel=1
pkgdesc="An advanced BitTorrent client programmed in C++, based on Qt toolkit and libtorrent-rasterbar (mingw-w64)"
arch=('any')
mingw_arch=('clang32' 'clang64' 'clangarm64' 'mingw32' 'mingw64' 'ucrt64')
url="https://qbittorrent.org/"
license=('custom' 'GPL')
depends=("${MINGW_PACKAGE_PREFIX}-libtorrent-rasterbar"
         "${MINGW_PACKAGE_PREFIX}-openssl"
         "${MINGW_PACKAGE_PREFIX}-qt6-base"
         "${MINGW_PACKAGE_PREFIX}-qt6-svg"
         "${MINGW_PACKAGE_PREFIX}-zlib")
makedepends=("git"
             "${MINGW_PACKAGE_PREFIX}-boost"
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
    "cmake/Modules/CommonConfig.cmake"
}

pkgver() {
  cd "$srcdir/${_realname}"

  _tag=$(git tag -l --sort -v:refname | grep -E '^release-[0-9\.]+$' | head -n1)
  _rev=$(git rev-list --count $_tag..HEAD)
  _hash=$(git rev-parse --short HEAD)
  printf "%s.r%s.g%s" "$_tag" "$_rev" "$_hash" | sed 's/^release-//'
}

build() {
  cd "$srcdir/${_realname}"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    "${MINGW_PREFIX}/bin/cmake.exe" \
      -B "_build" \
      -DCMAKE_BUILD_TYPE=RelWithDebInfo \
      -DCMAKE_INSTALL_PREFIX="${MINGW_PREFIX}" \
      -DCMAKE_INTERPROCEDURAL_OPTIMIZATION=ON \
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
