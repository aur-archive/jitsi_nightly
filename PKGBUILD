# Maintainer: Joris Steyn <jorissteyn@gmail.com>
# Contributors: Prurigro, Keshav P R, atommix aka Aleks Lifey, Xavion
# Contributors: Ananda Samaddar, Dan Serban, Xavier Devlamynck, David Zaragoza
# Package updated by: Ḷḷumex03
pkgname=jitsi_nightly
_pkgname=jitsi
# pkgver is determined automatically
pkgver=2014133
# pkgvermajor needs manual increment
_pkgvermajor=2.5
pkgrel=1
pkgdesc="An audio/video SIP VoIP phone and instant messenger written in Java (formerly SIP-Communicator)"
arch=('i686' 'x86_64')
url="http://jitsi.org"
license=('LGPL')
provides=(jitsi)
conflicts=(jitsi jitsi-stable jitsi-nightly)
depends=('java-environment' 'bash' 'gtk2' 'libxv' 'alsa-lib' 'libxss')
makedepends=('apache-ant')
optdepends=(
  'libpulse: PulseAudio support'
)
options=(!strip !emptydirs zipman !libtool docs)
source=(
  http://download.jitsi.org/jitsi/nightly/src/jitsi-src-${_pkgvermajor}-nightly.latest.zip
  ${_pkgname}.desktop
  ${_pkgname}.sh
)
md5sums=(
  'SKIP'
  'f5c21e511756458172dc0ae8020c7bd5'
   'c73e041d22fd1df76b60002b148af988'
)
install=jitsi_nightly.install

pkgver() {
  date +%Y%m%d
}

build() {
  cd "${srcdir}/jitsi"

  # append the build revision to the jitsi version
  sed -i "s/BUILD_ID="\"".*"\""/BUILD_ID="\"${pkgver}\""/" src/net/java/sip/communicator/impl/version/NightlyBuildID.java
  . /etc/profile.d/apache-ant.sh
  ant rebuild
}

package() {
  cd "${srcdir}/jitsi"
  find lib/ lib/bundle/ -maxdepth 1 -type f -exec install -Dm644 {} "${pkgdir}/usr/lib/jitsi/"{} \;
  shopt -sq extglob
  find lib/native/linux$(sed 's/_/-/g' <<<${CARCH/#*(i?86|x86)/})/ -maxdepth 1 -type f -execdir install -Dm644 {} "${pkgdir}/usr/lib/jitsi/lib/native/"{} \;
  find sc-bundles/{,os-specific/linux/} -maxdepth 1 -type f -execdir install -Dm644 {} "${pkgdir}/usr/lib/jitsi/sc-bundles/"{} \;
  install -Dm755 "$srcdir/jitsi.sh" "${pkgdir}/usr/bin/jitsi"
  install -Dm644 "$srcdir/jitsi.desktop" "${pkgdir}/usr/share/applications/jitsi.desktop"
  local _file
  for _file in resources/install/debian/*.{svg,xpm}; do
      install -Dm644 "$_file" "${pkgdir}/usr/share/pixmaps/jitsi${_file/*sip-communicator/}"
  done
}

# vim:set ts=2 sw=2 et:
