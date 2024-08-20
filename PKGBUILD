# Maintainer: NextWorks <nextworks@protonmail.com>
# Maintainer: Peter Jung <admin@ptr1337.dev>
# Contributor: Alad Wenter <alad@archlinux.org>
# Contributor: Luna Jernberg <droidbittin@gmail.com>
# Contributor: Hilton Medeiros <medeiros.hilton@gmail.com>
# Contributor: Simon Brulhart <simon@brulhart.me>
# Contributor: Det <nimetonmaili g-mail>, Achilleas Pipinellis, speed145a, Schnouki, aus

pkgname=zen-browser-bin
_pkgname=zen-browser
realpkgver=1.0.0-a.23
pkgver=1.0.0.a.23
pkgrel=1
pkgdesc="Standalone web browser - Static binaries from upstream"
arch=('x86_64' 'i686')
url="https://github.com/zen-browser/desktop"
license=(MPL-2.0)
depends=(gtk3 libxt mime-types dbus-glib nss ttf-font)
optdepends=('ffmpeg: H264/AAC/MP3 decoding'
            'networkmanager: Location detection via available WiFi networks'
            'libnotify: Notification integration'
            'pulseaudio: Audio support'
            'speech-dispatcher: Text-to-Speech'
            'hunspell-en_US: Spell checking, American English')
options=(!strip)
provides=("zen-browser=$pkgver")
conflicts=('zen-browser')

source=("zen-browser-$realpkgver.tar.bz2::https://github.com/zen-browser/desktop/releases/download/$realpkgver/zen.linux-generic.tar.bz2"
        "$_pkgname.sh"
        "$_pkgname.desktop"
        "policies.json")
sha256sums=('63c7f191eb3649dd9cee21a6616e9b9303e21136b2c73e349fe1647975dc22be'
            '642bcde5b15fddb712d10ed53299781108a265432237ab27a96c5c5c489718db'
            '2c81f08da79315c1a9d0fcf3a41ae6aa8b4bbe162181f6a8c41f37df27eb9870'
            'ccf5105135118acb7d05a7f81383307e959a3b757eb7b645600b47bb7915b504')

pkgver() {
  echo "$realpkgver" | tr '-' '.'
}

package() {
  # Create directories
  mkdir -p "$pkgdir"/usr/bin
  mkdir -p "$pkgdir"/usr/share/applications
  mkdir -p "$pkgdir"/opt

  # Install
  cp -r zen/ "$pkgdir"/opt/$pkgname

  # Launchers
  install -m755 $_pkgname.sh "$pkgdir"/usr/bin/$_pkgname

  # Desktops
  install -m644 *.desktop "$pkgdir"/usr/share/applications/

  # Icons
  for i in 16x16 32x32 48x48 64x64 128x128; do
    install -d "$pkgdir"/usr/share/icons/hicolor/$i/apps/
    ln -s /opt/$pkgname/browser/chrome/icons/default/default${i/x*}.png \
          "$pkgdir"/usr/share/icons/hicolor/$i/apps/$_pkgname.png
  done

  # Use system-provided dictionaries
  ln -Ts /usr/share/hunspell "$pkgdir"/opt/$pkgname/dictionaries
  ln -Ts /usr/share/hyphen "$pkgdir"/opt/$pkgname/hyphenation

  # Use system certificates
  ln -sf /usr/lib/libnssckbi.so "$pkgdir"/opt/$pkgname/libnssckbi.so

  # Disable update checks (managed by pacman)
  mkdir "$pkgdir"/opt/$pkgname/distribution
  install -m644 "$srcdir"/policies.json "$pkgdir"/opt/$pkgname/distribution/
}
