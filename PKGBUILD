# Maintainer: Dan Maftei <dan.maftei@gmail.com>
pkgname="molden"
pkgver=6.7
pkgrel=3
pkgdesc="A program for molecular and electronic structure visualization"
arch=('i686' 'x86_64')
url="http://www.cmbi.ru.nl/molden/"
license=('custom')
groups=()
depends=('mesa' 'glu')
makedepends=(
    'vi'
    'gcc-fortran'
    'xorgproto'
    'libx11'
    'mesa'
    'glu'
)
optdepends=(
   'openbabel: to create 2D images of the molecules in a .sdf file'
   'wget: to fetch PDB from rcsb.org'
)
provides=('molden')
conflicts=()
replaces=()
backup=()
options=()
install=${pkgname}.install
changelog=
source=(
    "ftp://ftp.cmbi.umcn.nl/pub/molgraph/molden/$pkgname$pkgver.tar.gz"
    "molden.desktop"
    "molden.png"
    "hacks.patch"
)
noextract=()
md5sums=('65d2f8fbdb2800e48bb0e12c814dfcd5' 'SKIP' 'SKIP' 'SKIP')
sha256sums=('ebd73e8d95271eb82a1464a7eab28a042662483bbff6e6dcc7db0d1c9b2e4432'
            '28b0a93a583f1c0d5e735964f9991bbfa18995709fd8d00dc9d91d619053d61c'
            'dc3104c3c3a9d437128865f0703258692e6727fc2cfa9718c7a3547d63c2bb31'
            'fbc92c4363e46a54edd7edee509f14c8ed399ca27320b3848afc2fac2a0148fd')

prepare () {
  cd "molden$pkgver"
  patch -Np1 -i "${srcdir}/hacks.patch"
}

build() {
  cd "molden$pkgver"
  # Patch Makefile for surf utility to reflect
  # the replacement of missing makedepend
  sed -i 's/@.*makedepend.*$/@ \$(CC) \$(INCLUDE) -M \$(SRCS) \> makedep/' src/surf/Makefile

  # Patch to compile with gfortran 10
  # Contributed by Panadestein on 5/31/2020
  sed -i 's/FFLAGS = -g ${AFLAG}/& -fallow-argument-mismatch/g' makefile
  sed -i 's/FFLAGS = -c -g -ffast-math -funroll-loops -O3/& -fallow-argument-mismatch/g' src/ambfor/makefile
  make
}

package() {
  cd "molden$pkgver"
  install -t "$pkgdir/usr/bin/"  -Dm755 bin/{molden,gmolden,ambfor,ambmd,surf}
  install -t "$pkgdir/usr/share/doc/$pkgname" -Dm755 doc/figures.ps.Z  doc/manual.ps.Z doc/manual.txt.Z
  install -t "$pkgdir/usr/share/licenses/$pkgname/" -Dm755 CopyRight COMMERCIAL_LICENSE REGISTER

  # install desktop file and icon
  install -m755 -d "$pkgdir"/usr/share/applications
  install -m644 -t "$pkgdir"/usr/share/applications/ ../$pkgname.desktop
  install -m755 -d "$pkgdir"/usr/share/pixmaps
  install -m644 -t "$pkgdir"/usr/share/pixmaps/ ../$pkgname.png
}

