# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=2017.2
_pkgver=172.3317.76
pkgrel=1
epoch=2
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="https://www.jetbrains.com/idea/"
license=('Apache')
backup=('usr/share/intellijidea-ce/bin/idea.vmoptions'
        'usr/share/intellijidea-ce/bin/idea64.vmoptions')
depends=('java-environment' 'giflib' 'libxtst' 'libxft' 'ttf-font'
         'coreutils' 'grep' 'which')
conflicts=('intellij-idea-libs')
replaces=('intellij-idea-libs')
install=idea.install
source=("http://download.jetbrains.com/idea/ideaIC-$pkgver-no-jdk.tar.gz"
        idea.desktop)
sha256sums=('d88257b17447398ed60e7d774803cbc21e0fe7750add09622a98b936f14cd91f'
            'bd37ad47c926941108f624cbe5adbd7fe91d198b15aca63d8a0c0da14c7a76a6')

package() {
  install -d -m755 "$pkgdir/"usr/share
  cp -a "idea-IC-$_pkgver" "$pkgdir"/usr/share/intellijidea-ce

  # make sure that all files are owned by root
  chown -R root:root "$pkgdir/usr/share"

  # never wait on user input when starting idea
  sed -i '/.*read IGNORE.*/ d' "$pkgdir"/usr/share/intellijidea-ce/bin/idea.sh

  install -d -m755 "$pkgdir"/usr/bin
  ln -s /usr/share/intellijidea-ce/bin/idea.sh "$pkgdir"/usr/bin/idea.sh
  install -D -m644 "$srcdir"/idea.desktop "$pkgdir"/usr/share/applications/idea.desktop
  install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea.png \
                   "$pkgdir"/usr/share/pixmaps/idea.png

  # workaround FS#40934
  sed -i 's|lcd|on|'  "$pkgdir"/usr/share/intellijidea-ce/bin/*.vmoptions
}

# vim:set ts=2 sw=2 et:
