# Maintainer: Hilinus

pkgname=aegisub-stable-svn
_pkgname=aegisub
pkgver=5438
pkgrel=2
pkgdesc="A general-purpose subtitle editor with ASS/SSA support"
arch=('i686' 'x86_64')
url="http://www.aegisub.net"
license=('GPL' 'BSD')
depends=('ffmpeg' 'lua' 'openal' 'wxgtk>=2.8.11' 'hunspell' 'libidn')
makedepends=('imagemagick>=6.6.2.10' 'subversion' 'intltool')
provides=('aegisub')
conflicts=('aegisub')
source=(license.txt)
md5sums=('3e13350007702bd7117e8f35bac376f1')

_svntrunk=http://svn.aegisub.org/branches/aegisub_2.1.9/aegisub
_svnmod=aegisub


build() {
  cd "$srcdir"
  
  if [ -d $_svnmod ]; then
    cd $_svnmod && svn up
  else
    svn co $_svntrunk $_svnmod
  fi

  if [ -d "$srcdir/$_svnmod-build" ]; then
    rm -rf "$srcdir/$_svnmod-build"
  fi

  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  
  cd "$srcdir/$_svnmod-build"
  
  ICONV_LIBS="-lidn" ./autogen.sh --prefix=/usr \
  --with-player-audio=openal --without-{portaudio,alsa,oss}

  make
}

package() {
  cd "${srcdir}/$_svnmod-build"
  make DESTDIR="$pkgdir" install

  # menu icon and mimetype fix
  sed -i -e 's/Icon=aegisub/Icon=\/usr\/share\/icons\/hicolor\/scalable\/apps\/aegisub.svg/' \
	-e 's/application\/x-srt/application\/x-subrip/' \
	 "$pkgdir"/usr/share/applications/aegisub.desktop

  # install the BSD license, although it is ruled by GPL according to the wiki:
  # (http://www.malakith.net/aegiwiki/Subtitling_software_comparison)
  install -D -m644 "$srcdir"/license.txt \
      "$pkgdir"/usr/share/licenses/$_pkgname/license.txt
}
