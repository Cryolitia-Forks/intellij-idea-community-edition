# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=14
_pkgver=139.224.1
pkgrel=1
epoch=1
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="http://www.jetbrains.org/"
license=('Apache')
depends=('java-environment' "intellij-idea-libs" 'giflib' 'libxtst')
install=idea.install
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver.tar.gz \
        idea.desktop idea.sh)
md5sums=('d5aeaefabfc47d40af7e8eb20068830a'
         '074bca11876cd1129e74feafd6271670'
         'f27bad35ee8e6445ca2f8a591bca895a')

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

  install -D -m755 "$srcdir"/idea.sh "$pkgdir"/usr/bin/idea.sh
  install -D -m644 "$srcdir"/idea.desktop "$pkgdir"/usr/share/applications/idea.desktop
  install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea.png \
                   "$pkgdir"/usr/share/pixmaps/idea.png


  # workaround FS#40934
  sed -i 's|lcd|on|'  "$pkgdir"/usr/share/intellijidea-ce/bin/*.vmoptions
}

# vim:set ts=2 sw=2 et:
