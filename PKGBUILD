# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=11.1.3
_pkgver=117.798
pkgrel=1
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="http://www.jetbrains.org/"
license=('apache')
depends=('java-environment' "intellij-idea-libs=$pkgver" 'giflib')
install=idea.install
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver.tar.gz \
        idea.desktop idea.sh)
md5sums=('b8ae865e3645cfd0a8d16dac481fd25c'
         '29e2d4ab0578a6d44533292bec8843ee'
         'f27bad35ee8e6445ca2f8a591bca895a')

build() {
  cd "$srcdir"

  install -d -m755 "$pkgdir/"usr/share
  cp -a "idea-IC-$_pkgver" "$pkgdir"/usr/share/intellijidea-ce

  # remove files owned by intellij-idea-libs
  rm "$pkgdir"/usr/share/intellijidea-ce/bin/{fsnotifier,libbreakgen}*

  # make sure that all files are owned by root
  chown -R root:root "$pkgdir/usr/share"

  # never wait on user input when starting idea
  sed -i '/.*read IGNORE.*/ d' "$pkgdir"/usr/share/intellijidea-ce/bin/idea.sh

  install -D -m755 "$srcdir"/idea.sh "$pkgdir"/usr/bin/idea.sh
  install -D -m644 "$srcdir"/idea.desktop "$pkgdir"/usr/share/applications/idea.desktop
  install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea.png \
                   "$pkgdir"/usr/share/pixmaps/idea.png
}

# vim:set ts=2 sw=2 et:
