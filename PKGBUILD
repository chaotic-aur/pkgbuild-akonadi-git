# Merged with official ABS akonadi PKGBUILD by João, 2021/11/28 (all respective contributors apply herein)
# Maintainer: João Figueiredo & chaotic-aur <islandc0der@chaotic.cx>
# Contributor: nl6720 <nl6720@archlinux.org>
# Contributor: Jack Random <jack (@) random.to>
# Contributor: Jerome Leclanche <jerome.leclanche+arch@gmail.com>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Antonio Rojas <arojas@archlinux.org
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Pierre Schmitz <pierre@archlinux.de>

pkgbase=akonadi-git
pkgname=(akonadi-git libakonadi-git)
pkgver=22.03.40_r12653.g114a6b91b
pkgrel=1
pkgdesc='PIM layer, which provides an asynchronous API to access all kind of PIM data'
arch=($CARCH)
url='https://kontact.kde.org'
license=(LGPL)
makedepends=(git extra-cmake-modules-git postgresql qt5-tools boost kitemmodels-git kaccounts-integration-git doxygen)
source=("git+https://github.com/KDE/${pkgname%-git}.git")
sha256sums=('SKIP')

pkgver() {
  cd ${pkgname%-git}
  _ver="$(grep -m1 'set(RELEASE_SERVICE_VERSION' CMakeLists.txt | cut -d '"' -f2 | tr - .)"
  _ver=${_ver:-"$(grep -m1 'set(PIM_VERSION' CMakeLists.txt | cut -d '"' -f2 | tr - .)"}
  echo "${_ver}_r$(git rev-list --count HEAD).g$(git rev-parse --short HEAD)"
}

build() {
  cmake -B build -S ${pkgbase%-git} \
    -DBUILD_TESTING=OFF \
    -DBUILD_QCH=ON
  cmake --build build
}

package_libakonadi-git() {
  pkgdesc='Libraries used by applications based on Akonadi'
  depends=(kitemmodels-git kaccounts-integration-git)
  provides=(libakonadi)

  DESTDIR="$pkgdir" cmake --install build
  rm -r "$pkgdir"/usr/bin # Provided by akonadi
}

package_akonadi-git() {
  depends=(libakonadi-git mariadb)
  optdepends=('postgresql: PostgreSQL backend')
  provides=(akonadi)

  DESTDIR="$pkgdir" cmake --install build
  rm -r "$pkgdir"/{etc,usr/{include,lib,share}} # Provided by libakonadi
}
