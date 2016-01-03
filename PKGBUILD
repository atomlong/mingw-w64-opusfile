# Maintainer: Dario Ostuni <another.code.996@gmail.com>
pkgname=mingw-w64-opusfile
pkgver=0.6
pkgrel=1
pkgdesc="The opusfile library provides seeking, decode, and playback of Opus streams in the Ogg container."
arch=(any)
url="http://www.opus-codec.org"
license=("BSD")
makedepends=(mingw-w64-gcc mingw-w64-pkg-config)
depends=(mingw-w64-crt mingw-w64-opus mingw-w64-libogg mingw-w64-openssl)
options=(staticlibs !strip !buildflags)
source=("http://downloads.xiph.org/releases/opus/opusfile-0.6.tar.gz")
sha256sums=("2428717b356e139f18ed2fdb5ad990b5654a238907a0058200b39c46a7d03ea6")

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    "${srcdir}"/${pkgname#mingw-w64-}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --disable-doc
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip --strip-unneeded
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}