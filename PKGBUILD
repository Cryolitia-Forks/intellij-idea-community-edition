# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=10.5.4
_pkgver=107.777
pkgrel=1
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="http://www.jetbrains.org/"
license=('apache')
depends=('java-environment' "intellij-idea-libs=$pkgver" 'giflib')
install=idea.install
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver.tar.gz \
        intellijidea.sh intellijidea.desktop intellijidea.png)
md5sums=('cf4fa1b51e9bc41c88323bfdf2147005'
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

  # never wait on user input when starting idea
  sed -i '/.*read IGNORE.*/ d' "$pkgdir"/usr/share/intellijidea-ce/bin/idea.sh

  install -D -m755 "$srcdir"/intellijidea.sh "$pkgdir"/usr/bin/intellijidea-ce
  install -D -m644 "$srcdir"/intellijidea.desktop "$pkgdir"/usr/share/applications/intellijidea-ce.desktop
  install -D -m644 "$srcdir"/intellijidea.png "$pkgdir"/usr/share/pixmaps/intellijidea-ce.png
}

# vim:set ts=2 sw=2 et:
