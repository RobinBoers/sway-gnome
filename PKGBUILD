# Maintainer: Robin Boers <robin@geheimesite.nl>
_pkgname="sway-gnome"
pkgname="$_pkgname-git"
pkgver=0.3.0.r3.ga96d119
pkgrel=1
pkgdesc="Opinionated Sway Configuration using GNOME session services, for GNOME >= 3.34"
arch=("x86_64")
url="https://github.com/RobinBoers/$_pkgname"
license=("MIT")
depends=(sway swaylock swayidle kanshi mako gnome-session xdg-desktop-portal polkit-gnome gnome-keyring gnome-settings-daemon clipman)
makedepends=(meson)
provides=("$_pkgname")
source=("$_pkgname::git+https://github.com/RobinBoers/$_pkgname#branch=master")
md5sums=('SKIP')

pkgver() {
  cd "$srcdir/$_pkgname"
  git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
  cd "$srcdir/$_pkgname"
  meson builddir && cd builddir
  meson compile
  DESTDIR="$pkgdir/" meson install
}
