# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=2016.2.5
_pkgver=162.2228.15
pkgrel=1
epoch=2
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="https://www.jetbrains.com/idea/"
license=('Apache')
backup=('usr/share/intellijidea-ce/bin/idea.vmoptions'
        'usr/share/intellijidea-ce/bin/idea64.vmoptions')
depends=('java-environment' "intellij-idea-libs" 'giflib' 'libxtst' 'libxft' 'ttf-font')
install=idea.install
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver-no-jdk.tar.gz \
        idea.desktop)
sha256sums=('400cdfb34d12dea85e9f946de3f3bef2d6d55264f6bf889027e859b0ea9cf124'
            'bd37ad47c926941108f624cbe5adbd7fe91d198b15aca63d8a0c0da14c7a76a6')

package() {
  install -d -m755 "$pkgdir/"usr/share
  cp -a "idea-IC-$_pkgver" "$pkgdir"/usr/share/intellijidea-ce

  # remove files owned by intellij-idea-libs
  rm "$pkgdir"/usr/share/intellijidea-ce/bin/{fsnotifier,libbreakgen}*
  rm -rf "$pkgdir"/usr/share/intellijidea-ce/lib/libpty

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
