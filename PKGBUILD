# $Id: PKGBUILD 148733 2012-02-05 11:48:34Z ibiru $
# Maintainer: tobias <tobias@archlinux.org>

pkgname=gimp26
pkgver=2.6.12
pkgrel=2
pkgdesc="GNU Image Manipulation Program"
arch=('i686' 'x86_64')
url="http://www.gimp.org/"
license=('GPL' 'LGPL')
depends=('pygtk' 'lcms' 'libxpm' 'libwmf' 'libxmu' 'librsvg' 'libmng' 'dbus-glib' \
         'libexif' 'gegl' 'desktop-file-utils' 'hicolor-icon-theme')
makedepends=('intltool' 'libwebkit' 'poppler-glib' 'alsa-lib' 'iso-codes' 'curl')
optdepends=('gutenprint: for sophisticated printing only as gimp has built-in cups print support'
            'libwebkit: for the help browser'
            'poppler-glib: for pdf support'
            'alsa-lib: for MIDI event controller module'
            'curl: for URI support')
options=('!libtool' '!makeflags')
conflicts=('gimp-devel')
install=gimp.install
source=(ftp://ftp.gimp.org/pub/gimp/v${pkgver%.*}/${pkgname//26}-${pkgver}.tar.bz2 linux.gpl 
        uri-backend-libcurl.patch)
sha1sums=('82964e3d4eb003239f3443a1bccac53f5d780e15'
          '110ce9798173b19a662d086ed7b882b4729f06cf'
          'a65b0ee6cd1b4345065b7b98c07f2fed15f844f4')

build() {
  cd "${srcdir}/${pkgname//26}-${pkgver}"
  patch -p1 < ../uri-backend-libcurl.patch
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
    --enable-mp --enable-gimp-console --enable-gimp-remote \
    --enable-python --with-gif-compression=lzw --with-libcurl \
    --without-aa --without-hal --without-gvfs --without-gnomevfs
  make
}

package() {
  cd "${srcdir}/${pkgname//26}-${pkgver}"
  make DESTDIR="${pkgdir}" install
  sed -i 's|#!/usr/bin/env python|#!/usr/bin/env python2|' "${pkgdir}"/usr/lib/gimp/2.0/plug-ins/*.py
  install -D -m644 "${srcdir}/linux.gpl" "${pkgdir}/usr/share/gimp/2.0/palettes/Linux.gpl"

  rm "${pkgdir}/usr/share/man/man1/gimp-console.1"
  ln -s gimp-console-${pkgver%.*}.1.gz "${pkgdir}/usr/share/man/man1/gimp-console.1.gz"
  ln -s gimptool-2.0 "${pkgdir}/usr/bin/gimptool"
  ln -sf gimptool-2.0.1.gz "${pkgdir}/usr/share/man/man1/gimptool.1.gz"
}
