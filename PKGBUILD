# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=2018.1.1
_pkgver=181.4445.78
pkgrel=1
epoch=2
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="https://www.jetbrains.com/idea/"
license=('Apache')
backup=('usr/share/intellijidea-ce/bin/idea.vmoptions'
        'usr/share/intellijidea-ce/bin/idea64.vmoptions')
depends=('java-environment=8' 'giflib' 'libxtst' 'libxft' 'ttf-font'
         'coreutils' 'grep' 'which')
conflicts=('intellij-idea-libs')
replaces=('intellij-idea-libs')
install=idea.install
source=("https://download.jetbrains.com/idea/ideaIC-${pkgver}-no-jdk.tar.gz"
        idea.desktop idea.sh)
sha256sums=('c882dee60b7927ca9b2ccb13a46cff4c3ecc688d7e7eec9c53878a801ab1d5b6'
            'bd37ad47c926941108f624cbe5adbd7fe91d198b15aca63d8a0c0da14c7a76a6'
            '0e5d6a47b5ae464e9f562110ccc798f55055943e425e0621c0275f72615fdb1d')

package() {
  install -d -m755 "$pkgdir/"usr/share
  cp -a "idea-IC-$_pkgver" "$pkgdir"/usr/share/intellijidea-ce

  # make sure that all files are owned by root
  chown -R root:root "$pkgdir/usr/share"

  install -d -m755 "$pkgdir"/usr/bin
  install -D -m755 "$srcdir"/idea.sh "$pkgdir"/usr/bin/idea.sh
  install -D -m644 "$srcdir"/idea.desktop "$pkgdir"/usr/share/applications/idea.desktop
  install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea.png \
                   "$pkgdir"/usr/share/pixmaps/idea.png

  # workaround FS#40934
  sed -i 's|lcd|on|'  "$pkgdir"/usr/share/intellijidea-ce/bin/*.vmoptions
}

# vim:set ts=2 sw=2 et:
