# Maintainer: laloch <lalochcz@gmail.com>
pkgname=monkeystudio2-svn
pkgver=4524
pkgrel=2
pkgdesc="A free crossplatform Qt4 IDE"
arch=('i686' 'x86_64')
url="http://monkeystudio.org"
license=('GPL')
depends=('qscintilla' 'python')
makedepends=('subversion')
provides=('monkeystudio2')
conflicts=('monkeystudio' 'monkeystudio2' 'kdemod-monkeystudio2' 'kdemod-monkeystudio2-svn')
options=('!emptydirs')
#install=
#source=()
#md5sums=()

_svntrunk=svn://svn.tuxfamily.org/svnroot/monkeystudio/mks/v2/branches/dev
_svnmod=monkeystudio2-svn

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  sed -i 's/qt warn_on thread x11 windows rtti debug/qt warn_on thread x11 windows rtti release/g' ${srcdir}/${_svnmod}/config.pri
  sed -i 's/warn_off release debug_and_release x86 x86_64 ppc ppc64/warn_off debug_and_release x86 x86_64 ppc ppc64/g' ${srcdir}/${_svnmod}/config.pri
  
  qmake-qt4 -r ${srcdir}/${_svnmod} prefix=/usr plugins=/usr/lib
  make
  qmake-qt4 -r ${srcdir}/${_svnmod} prefix=/usr plugins=/usr/lib
}

package() {
  cd "$srcdir/$_svnmod-build"
  make INSTALL_ROOT="$pkgdir/" install
}

# vim:set ts=2 sw=2 et:
