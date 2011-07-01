# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=10.5.1
_pkgver=107.322
pkgrel=1
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="http://www.jetbrains.org/"
license=('apache')
depends=('jdk' "intellij-idea-libs=$pkgver")
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver.tar.gz \
        intellijidea.sh intellijidea.desktop intellijidea.png)
md5sums=('f90ce6b7a2b8d2191156dd111fe6a63d'
         'a4d631a3bda1828a9d520c458f516e37'
         '80e0bfcb5731a4426f165d120acd9164'
         'f22815c48dca91edc530953d5aa806dd')

build() {
  cd "$srcdir"

  install -d -m755 "$pkgdir/"usr/{bin,share}
  cp -a "idea-IC-$_pkgver" "$pkgdir"/usr/share/intellijidea-ce

  # remove files owned by intellij-idea-libs
  rm "$pkgdir"/usr/share/intellijidea-ce/bin/{fsnotifier,libbreakgen}*

  # make sure that all files are owned by root
  chown -R root:root "$pkgdir/usr/share"

  install -D -m755 "$srcdir"/intellijidea.sh "$pkgdir"/usr/bin/intellijidea-ce
  install -D -m644 "$srcdir"/intellijidea.desktop "$pkgdir"/usr/share/applications/intellijidea-ce.desktop
  install -D -m644 "$srcdir"/intellijidea.png "$pkgdir"/usr/share/pixmaps/intellijidea-ce.png
}

# vim:set ts=2 sw=2 et:
