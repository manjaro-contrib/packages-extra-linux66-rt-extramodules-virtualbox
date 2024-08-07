# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Archlinux credits:
# Ionut Biru <ibiru@archlinux.org>
# Sébastien Luttringer <seblu@aur.archlinux.org>

_linuxprefix=linux66-rt
_extramodules=extramodules-6.6-rt-MANJARO
_pkgname=virtualbox-host-modules
pkgname=$_linuxprefix-$_pkgname
pkgver=7.0.20
_pkgver="${pkgver}_OSE"
pkgrel=6
arch=('x86_64')
url='http://virtualbox.org'
license=('GPL')
pkgdesc='Host kernel modules for VirtualBox'
groups=("$_linuxprefix-extramodules")
install=virtualbox-host-modules.install
depends=("$_linuxprefix")
makedepends=("virtualbox-host-dkms>=$pkgver" 'dkms' "$_linuxprefix" "$_linuxprefix-headers")
provides=('VIRTUALBOX-HOST-MODULES')
replaces=("linux515-rt-$_pkgname" "linux60-rt-$_pkgname")

build() {
  _kernver="$(cat /usr/lib/modules/$_extramodules/version)"

  # build host modules
  echo 'Host modules'
  fakeroot dkms build --dkmstree "$srcdir" -m vboxhost/${pkgver}_OSE -k ${_kernver}
}

package(){
  _kernver="$(cat /usr/lib/modules/$_extramodules/version)"

  cd "vboxhost/${pkgver}_OSE/$_kernver/$CARCH/module"
  install -Dm644 * -t "$pkgdir/usr/lib/modules/$_extramodules/"

  # compress each module individually
  find "$pkgdir" -name '*.ko' -exec xz -T1 {} +

  # systemd module loading
  printf '%s\n' vboxdrv vboxnetadp vboxnetflt |
  install -Dm644 /dev/stdin "$pkgdir/usr/lib/modules-load.d/$pkgname.conf"
}
