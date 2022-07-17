# Maintainer: Robin Boers <robin@geheimesite.nl>
pkgname=sway-gnome-git
pkgver=1
pkgrel=1
pkgdesc=" Opinionated Sway Configuration using GNOME session services, for GNOME >= 3.34 "
arch=('x86_64')
url="https://github.com/RobinBoers/sway-gnome"
license=('MIT')
depends=(sway swaylock swayidle kanshi mako gnome-session xdg-desktop-portal polkit-gnome gnome-keyring gnome-settings-deamon)
makedepends=(meson)
provides=(sway-gnome)
source=('sway-gnome::git+https://github.com/RobinBoers/sway-gnome#branch=master')

pkgver() {
  cd "$pkgname"
  git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  cd "$pkgname"
  meson builddir && cd builddir
  meson compile
}

package() {
  cd "$pkgname"
  DESTDIR="$pkgdir/" meson install
}
