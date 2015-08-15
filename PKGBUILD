# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>
# based on a PKGBUILD of Mathias Nedrebø <mathias <at> nedrebo.org>

pkgname=emacs-xwidget-bzr
pkgver=101558
pkgrel=1
arch=('i686' 'x86_64')
pkgdesc="Emacs, bazaar-version from the xwidget branch"
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL3')
depends=('alsa-lib' 'gpm' 'desktop-file-utils' 
  'hicolor-icon-theme' 'webkitgtk3' 'm17n-lib' 'giflib' 
  'gobject-introspection' 'imagemagick' 'librsvg' 'gconf')
makedepends=('bzr' 'texinfo' 'glproto')
options=('docs' '!emptydirs' '!makeflags')
provides=('emacs=24.4.50.1' 'cedet')
conflicts=('emacs')
install=emacs.install
backup=('usr/share/applications/emacs.desktop' 
  'usr/share/emacs/site-lisp/subdirs.el')
source=("bzr+http://bzr.savannah.gnu.org/r/emacs/xwidget/")
md5sums=('SKIP')
_bzrmod=xwidget

pkgver() {
  cd $srcdir/${_bzrmod}
  bzr revno
}

build() {
  cd $srcdir/${_bzrmod}
  export LDFLAGS="`pkg-config --libs MagickWand `"
  ./autogen.sh && \
      ac_cv_lib_gif_EGifPutExtensionLast=yes ./configure \
					--prefix=/usr \
					--with-xwidgets \
					--with-x-toolkit=gtk3 \
					--with-sound \
					--libexecdir=/usr/lib \
					--localstatedir=/var \
					--with-rsvg \
					--with-gconf \
					--with-gif
  make bootstrap 
}

package() {
  cd $srcdir/${_bzrmod}
  make DESTDIR=$pkgdir install
  # adding the READMEs. 
  install -Dm644 $srcdir/${_bzrmod}/README \
	  $pkgdir/usr/share/doc/$pkgname/README
  install -Dm644 $srcdir/${_bzrmod}/README.xwidget \
	  $pkgdir/usr/share/doc/$pkgname/README.xwidget
  # remove conflict with ctags package
  mv "$pkgdir"/usr/bin/{ctags,ctags.emacs}
  mv "$pkgdir"/usr/share/man/man1/{ctags.1.gz,ctags.emacs.1.gz}
  # remove conflict with the texinfo-package
  rm $pkgdir/usr/share/info/info.info.gz
  # Adjust permissions
  find "${pkgdir}"/usr/share/emacs -type d -exec chmod 755 {} \;
  find "${pkgdir}"/usr/share/emacs -exec chown root.root {} \;
  
  # fix perms on /var/games
  chgrp -R games "$pkgdir"/var/games
  chmod 775 "$pkgdir"/var/games
  chmod 775 "$pkgdir"/var/games/emacs
  chmod 664 "$pkgdir"/var/games/emacs/*
}
