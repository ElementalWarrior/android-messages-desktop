# Maintainer: Mark Wagie <mark dot wagie at tutanota dot com>
pkgname=android-messages-desktop
pkgver=5.4.1
pkgrel=4
_electronversion=21
pkgdesc="Android Messages as a cross-platform desktop app"
arch=('x86_64')
url="https://github.com/LanikSJ/android-messages-desktop"
license=('MIT')
depends=("electron${_electronversion}")
makedepends=('git' 'yarn')
source=("$pkgname-$pkgver.tar.gz::$url/archive/refs/heads/master.tar.gz"
        "$pkgname.sh"
        "$pkgname.desktop")
sha256sums=('0869dd24a3b845f9e6a5b11041f9f42af0d6d2494079d6391208891f1a23dafc'
            'eadbe21598990d35dcf9c475b5577fd06091d3507e2847e5d4d2288735203051'
            '1bf16b8864712b0c1de72d8c3764db14b75ecf64dae44d206a26aa036ac53b1a')

build() {
  # cd "$pkgname-master"
  electronDist="/usr/lib/electron${_electronversion}"
  electronVer="$(sed s/^v// /usr/lib/electron${_electronversion}/version)"
  export YARN_CACHE_FOLDER="$srcdir/yarn-cache"
  yarn install
  yarn webpack --mode=development
  yarn electron-builder --linux --x64 --dir \
    $dist -c.electronDist=$electronDist -c.electronVersion=$electronVer
}

package() {
  # cd "$pkgname-master"
  srcdir=`pwd`
  install -Dm644 dist/linux-unpacked/resources/app.asar -t "$pkgdir/usr/lib/$pkgname/resources/"

  install -Dm755 "$srcdir/$pkgname.sh" "$pkgdir/usr/bin/$pkgname"
  install -Dm644 "$srcdir/$pkgname.desktop" -t "$pkgdir/usr/share/applications"
  install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname"

  for icon_size in 16 24 32 48 64 128 256 512 1024; do
    icons_dir=/usr/share/icons/hicolor/${icon_size}x${icon_size}/apps
    install -Dm644 resources/icons/${icon_size}x${icon_size}.png \
      $pkgdir$icons_dir/$pkgname.png
  done
}
