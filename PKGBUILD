# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
pkgname=intellij-idea-community-edition
pkgver=11.0.2
_pkgver=111.277
pkgrel=1
pkgdesc="IDE for Java, Groovy and other programming languages with advanced refactoring features"
arch=('any')
url="http://www.jetbrains.org/"
license=('apache')
depends=('java-environment' "intellij-idea-libs=$pkgver" 'giflib')
install=idea.install
source=(http://download.jetbrains.com/idea/ideaIC-$pkgver.tar.gz \
        idea_CE.desktop idea_CE.sh)
md5sums=('156fddbdeba44bb9427c05fe567f7070'
         'cd6dedad6156dbaf41a32641e8b7f2cc'
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

  install -D -m755 "$srcdir"/idea_CE.sh "$pkgdir"/usr/bin/idea_CE
  install -D -m644 "$srcdir"/idea_CE.desktop "$pkgdir"/usr/share/applications/idea_CE.desktop
  install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea_CE48.png \
                   "$pkgdir"/usr/share/pixmaps/idea_CE.png
  _ICONS=('16' '32' '48' '128')
  for res in ${_ICONS[@]}; do
    install -D -m644 "$pkgdir"/usr/share/intellijidea-ce/bin/idea_CE${res}.png \
                     "$pkgdir"/usr/share/icons/hicolor/${res}x${res}/apps/idea_CE.png
  done
}

# vim:set ts=2 sw=2 et:
