# Maintainer: Luis Vervaet <luisvervaet@gmail.com>
# Maintainer: Peter Jung <admin@ptr1337.dev>
# Maintainer: Omansh Krishn <omansh@duck.com>
# Contributor: NextWorks <nextworks@protonmail.com>
# Contributor: Alad Wenter <alad@archlinux.org>
# Contributor: Luna Jernberg <droidbittin@gmail.com>
# Contributor: Hilton Medeiros <medeiros.hilton@gmail.com>
# Contributor: Simon Brulhart <simon@brulhart.me>
# Contributor: Det <nimetonmaili g-mail>, Achilleas Pipinellis, speed145a, Schnouki, aus

pkgname=zen-browser-twilight-bin
_pkgname=zen-browser-twilight
_name=zen-twilight
pkgver=1.15t.20250727.230742
pkgrel=1
pkgdesc="Performance oriented Firefox-based web browser - Twilight"
arch=('x86_64' 'aarch64')
url="https://github.com/zen-browser/desktop"
license=(MPL-2.0)
depends=(gtk3 libxt mime-types dbus-glib nss ttf-font systemd)
optdepends=('ffmpeg: H264/AAC/MP3 decoding'
            'networkmanager: Location detection via available WiFi networks'
            'libnotify: Notification integration'
            'pulse-native-provider: Audio support'
            'speech-dispatcher: Text-to-Speech'
            'hunspell-en_US: Spell checking, American English')
options=(!strip !debug)
provides=("zen-browser-twilight" "zen-browser-twilight=${pkgver}")
conflicts=("zen-twilight" "zen-twilight-bin")

source=("https://github.com/zen-browser/desktop/releases/download/twilight/zen.linux-x86_64.tar.xz"
        "${_name}.desktop::https://raw.githubusercontent.com/zen-browser/desktop/refs/tags/twilight/AppDir/zen.desktop"
        "policies.json")
sha256sums=('SKIP'
            'SKIP'
            'f93eb77db526147a8a20744905923a6eda79e2fbcc9f282e2f9228a7a995c798')

pkgver() {
  curl -s https://api.github.com/repos/zen-browser/desktop/releases/tags/twilight | jq -r '.name' | sed -E 's/.*- ([0-9.]+t) \(([0-9]{4})-([0-9]{2})-([0-9]{2}) at ([0-9]{2}):([0-9]{2}):([0-9]{2})\)/\1.\2\3\4.\5\6\7/'
}

package() {
  # Create directories
  mkdir -p ${pkgdir}/usr/bin
  mkdir -p ${pkgdir}/usr/share/applications
  mkdir -p ${pkgdir}/opt

  # Install
  cp -r zen/ ${pkgdir}/opt/${_pkgname}

  # Launchers
  mv ${pkgdir}/opt/${_pkgname}/zen ${pkgdir}/opt/${_pkgname}/${_name}
  ln -s /opt/${_pkgname}/${_name} ${pkgdir}/usr/bin/${_name}

  # Desktop
  sed -i "s|Name=Zen Browser|Name=Zen Twilight|" ${_name}.desktop
  sed -i "s|Exec=zen|Exec=${_name}|" ${_name}.desktop
  sed -i "s|Icon=zen|Icon=${_name}|" ${_name}.desktop
  install -m644 ${_name}.desktop ${pkgdir}/usr/share/applications/

  # Icons
  for i in 16x16 32x32 48x48 64x64 128x128; do
    install -d "${pkgdir}"/usr/share/icons/hicolor/${i}/apps/
    ln -s /opt/${_pkgname}/browser/chrome/icons/default/default${i/x*}.png ${pkgdir}/usr/share/icons/hicolor/${i}/apps/${_name}.png
  done

  # Use system-provided dictionaries
  ln -Ts /usr/share/hunspell ${pkgdir}/opt/${_pkgname}/dictionaries
  ln -Ts /usr/share/hyphen ${pkgdir}/opt/${_pkgname}/hyphenation

  # Use system certificates
  ln -sf /usr/lib/libnssckbi.so ${pkgdir}/opt/${_pkgname}/libnssckbi.so

  # Disable update checks (managed by pacman)
  mkdir ${pkgdir}/opt/${_pkgname}/distribution
  install -m644 ${srcdir}/policies.json ${pkgdir}/opt/${_pkgname}/distribution/
}
