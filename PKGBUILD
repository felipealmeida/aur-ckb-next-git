# Maintainer: Tasos Sahanidis <aur@tasossah.com>
# Contributor: makz <contact+aur@makz.me>
# Contributor: Light2Yellow <oleksii.vilchanskyi@gmail.com>

pkgname=ckb-next-git
pkgver=0.4.2.r58.g26bea40
pkgrel=1
epoch=1
pkgdesc="Corsair Keyboard and Mouse Input Driver, git master branch"
arch=('i686' 'x86_64')
url="https://github.com/ckb-next/ckb-next"
license=('GPL2')
depends=('qt5-base' 'hicolor-icon-theme' 'quazip' 'qt5-tools' 'libxcb' 'xcb-util-wm' 'qt5-x11extras')
makedepends=('git' 'cmake')
optdepends=('libappindicator-gtk2: Ayatana indicators in Unity, KDE or Systray (GTK+ 2 library)' 'libpulse')
conflicts=('ckb-git' 'ckb-git-latest' 'ckb-next')
provides=('ckb-next')
install=ckb-next-git.install
source=('ckb-next-git::git+https://github.com/felipealmeida/ckb-next.git#branch=non-gui-background-mode'
        ckbnext.tmpfiles
        ckbnext.sysusers
        ckb-next.service)
md5sums=('SKIP'
        1d67d2381b1c31365c4da85860917027
        94098573b6d2767bd843d3f85c94bf18
        2aae1795e211ce216a8fb9a28386124f)

pkgver() {
  cd "$srcdir/${pkgname%-VCS}"
  # based only on tags, always has long version as it's a rolling release
  # discards v, replaces dashes with dots
  git describe --tags --long | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}


build() {
  cd "$srcdir/${pkgname%-VCS}"

  cmake -H. -Bbuild                               \
    -DCMAKE_INSTALL_PREFIX="/usr"                 \
    -DCMAKE_BUILD_TYPE="Release"                  \
    -DCMAKE_INSTALL_LIBDIR="lib"                  \
    -DCMAKE_INSTALL_LIBEXECDIR="lib"              \
    -DDISABLE_UPDATER=1                           \
    -DUDEV_RULE_DIRECTORY="/usr/lib/udev/rules.d" \
    -DFORCE_INIT_SYSTEM="systemd"
  cmake --build build --target all
}

package() {
  cd "$srcdir/${pkgname%-VCS}"

  DESTDIR="$pkgdir" cmake --build build --target install
  install -Dm0644 "$srcdir"/ckbnext.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/ckbnext.conf
  install -Dm0644 "$srcdir"/ckbnext.sysusers "$pkgdir"/usr/lib/sysusers.d/ckbnext.conf
  install -Dm0644 "$srcdir"/ckb-next.service "$pkgdir"/usr/lib/systemd/system/ckb-next.service
}
