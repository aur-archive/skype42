# Contributor: Jaroslaw Swierczynski <swiergot@aur.archlinux.org>
# Contributor: Daenyth <Daenyth+Arch AT gmail DOT com>
# Maintainer: mid-kid <esteve.varela@gmail.com>

_pkgname=skype
pkgname=skype42
pkgver=4.2.0.13
pkgrel=7
arch=(i686 x86_64)
pkgdesc="P2P software for high-quality voice communication"
url="http://www.skype.com/"
license=('custom')
options=('!strip')
install=skype.install
depends=(xdg-utils hicolor-icon-theme)
#makedepends=(bsdiff)
#conflicts=('skype-staticqt' 'skype')
provides=('skype')

if [[ $CARCH == i686 ]]; then
  depends+=(qt4 alsa-lib libxss libxv libxcursor v4l-utils gstreamer0.10-base)
  optdepends=('libcanberra: XDG sound support'
              'pulseaudio: PulseAudio support')
else
  depends+=(lib32-{qt4,alsa-lib,libxss,libxv,libxcursor,v4l-utils})
  optdepends=('lib32-libcanberra: XDG sound support'
              'lib32-libpulse: PulseAudio support')
#  conflicts+=(bin32-skype bin32-skype-staticqt)
  provides+=("bin32-skype=$pkgver")
#  replaces=(bin32-skype)
fi

source=(http://download.skype.com/linux/$_pkgname-$pkgver.tar.bz2 PERMISSION skype-wrapper skype-4.3.0.37-verno.patch
        https://sources.archlinux.org/other/skype/qtwebkit-2.2.2-i686.tar.xz)
md5sums=('b4d1dcc5290be92225b400ea877db874'
         '26e1772379d4d4df9471b6ed660a6d97'
         '934db800aa1cc39d6aa4270f6157dc2c'
         '8ed63b5081cbbf4bbb5f0c797d1a0a23'
         '42c01ffa98e32b59605403efb42c8821')

package() {
  cd "$srcdir/$_pkgname-$pkgver"

  if [[ $CARCH == i686 ]]; then
	  libdir="usr/lib"
  
      # FS#33417 - Skype wants qtwebkit 2.2.x
      install -Dm755 "${srcdir}"/libQtWebKit.so.4 "${pkgdir}"/usr/share/$pkgname/lib/libQtWebKit.so.4
  else
	  libdir="usr/lib32"
  fi

  install -d "$pkgdir/usr/bin"
  sed -e "s#@LIBDIR@#/$libdir#" -e "s#@PKGNAME@#$pkgname#" "$srcdir/skype-wrapper" > "$pkgdir/usr/bin/$pkgname"
  chmod 755 "$pkgdir/usr/bin/$pkgname"

  # Executable
#  bspatch skype skype-patched "$srcdir/skype-4.3.0.37-verno.patch"
#  install -D skype-patched "$pkgdir/$libdir/skype/skype"
  install -D skype "$pkgdir/$libdir/$pkgname/skype"

  # Data
  mkdir -p "$pkgdir"/usr/share/$pkgname/{avatars,lang,sounds}
  install -m 644 avatars/* "$pkgdir/usr/share/$pkgname/avatars"
  install -m 644 lang/*    "$pkgdir/usr/share/$pkgname/lang"
  install -m 644 sounds/*  "$pkgdir/usr/share/$pkgname/sounds"

  # DBus Service
  install -Dm 644 skype.conf \
    "$pkgdir/etc/dbus-1/system.d/$pkgname.conf"

  # Icons
  for _i in 16 32 48; do
    install -Dm 644 icons/SkypeBlue_${_i}x${_i}.png \
      "$pkgdir/usr/share/icons/hicolor/${_i}x${_i}/apps/$pkgname.png"
  done
  install -Dm 644 icons/SkypeBlue_48x48.png \
    "$pkgdir/usr/share/pixmaps/$pkgname.png"

  # Desktop file
  sed -i -e "s/skype %U/$pkgname %U/" -e "s/skype.png/$pkgname.png/" \
	 -e "s/Name=Skype/Name=Skype 4.2/" skype.desktop
  install -Dm 644 skype.desktop \
    "$pkgdir/usr/share/applications/$pkgname.desktop"

  # License
  install -Dm 644 LICENSE \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm 644 "$srcdir/PERMISSION" \
    "$pkgdir/usr/share/licenses/$pkgname/PERMISSION"
}
