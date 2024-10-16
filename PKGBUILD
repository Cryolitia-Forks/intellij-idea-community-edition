# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Maintainer: Orhun Parmaksız <orhun@archlinux.org>

pkgname=intellij-idea-community-edition
pkgver=2024.2.3
_build=242.23339.11
_jrever=21
_jdkver=21
pkgrel=1
epoch=4
pkgdesc='IDE for Java, Groovy and other programming languages with advanced refactoring features'
url='https://www.jetbrains.com/idea/'
arch=('x86_64')
license=('Apache')
backup=('usr/share/idea/bin/idea64.vmoptions')
depends=('giflib' 'jdk17-openjdk' "java-environment-openjdk=${_jrever}" 'python' 'sh' 'ttf-font' 'libdbusmenu-glib' 'fontconfig' 'hicolor-icon-theme')
makedepends=('ant' 'git' maven cargo cmake libx11) # libx11: header only
optdepends=(
  'lldb: lldb frontend integration'
)
source=("git+https://github.com/JetBrains/intellij-community.git#tag=idea/${_build}"
        idea-android::"git+https://github.com/JetBrains/android.git#tag=idea/${_build}"
        idea.desktop
        idea.sh
        enable-no-jdr.patch
        # The class src/com/intellij/openapi/projectRoots/ex/JavaSdkUtil.java:56 (git commit 0ea5972cdad569407078fb27070c80e2b9235c53)
        # assumes the user's maven repo is at {$HOME}/.m2/repository and it contains junit-3.8.1.jar
        https://repo1.maven.org/maven2/junit/junit/3.8.1/junit-3.8.1.jar
        intellij-riscv64.patch
        # Who knows which commit Jetbrain is using? The following commit works anyway.
        git+https://github.com/JetBrains/pty4j.git#commit=673a524230c1a46782211e77b1750877b3aa71f7)
noextract=('junit-3.8.1.jar')
sha256sums=('eedc27bb3714faed9222453ecc0887fd583bcca64fcdb51048f84c4ee5f2460a'
            'e8020b00170f53e50e783baa18ad1b0bddf0a68f9b1cc153c6a77bfaaf74ef49'
            '049c4326b6b784da0c698cf62262b591b20abb52e0dcf869f869c0c655f3ce93'
            '6f2b00d5bcf54639e6a1fa6387dffb9fb035f1dd58ffd31ea1689af74a30b784'
            'b7858737346fb08423ee7fd177f9e59180d2173d988dd8622b84d58426fcb3a7'
            'b58e459509e190bed737f3592bc1950485322846cf10e78ded1d065153012d70'
            SKIP
            SKIP)

case "$CARCH" in
  x86_64)
      _arch=amd64
      _alt_arch=x86-64
      _suffix=""
      ;;
  *)
      _arch="$CARCH"
      _alt_arch="$CARCH"
      _suffix="-$CARCH"
      ;;
esac

prepare() {
  cd intellij-community

  patch -Np1 -i ../intellij-riscv64.patch

  pushd native/restarter
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
  popd

  # build system doesn't like symlinks
  mv "${srcdir}"/idea-android android

  export MAVEN_REPOSITORY=${srcdir}/.m2/repository
  mvn install:install-file \
    -Dfile="${srcdir}"/junit-3.8.1.jar \
    -DgroupId=junit \
    -DartifactId=junit \
    -Dversion=3.8.1 \
    -Dpackaging=jar \
    -DgeneratePom=true

  patch -Np1 < "${srcdir}/enable-no-jdr.patch"
}

build() {
  pushd pty4j/native
  # That Makefile is a mess and only intended for cross. Let's build manually
  _cc="gcc $CFLAGS $CPPFLAGS -I.  -D_REENTRANT -D_GNU_SOURCE -fPIC"
  $_cc -c -o exec_pty.o exec_pty.c
  $_cc -c -o openpty.o openpty.c
  $_cc -c -o pfind.o pfind.c
  gcc $LDFLAGS $CFLAGS -g -shared -Wl,-soname,libpty.so -o libpty.so *.o
  popd

  cd intellij-community

  mkdir -p bin/linux/$_arch

  pushd native/restarter
  cargo build --frozen --release
  popd
  export CC=gcc
  pushd native/fsNotifier/linux
  ./make.sh
  cp fsnotifier ../../../bin/linux/$_arch
  popd

  pushd native/LinuxGlobalMenu
  cmake .
  make
  cp libdbm.so ../../bin/linux/$_arch
  popd

  pushd platform/sqlite
  OS=linux ARCH=$CARCH ./make.sh
  popd
  
  export JAVA_HOME="/usr/lib/jvm/java-${_jdkver}-openjdk"
  export PATH="/usr/lib/jvm/java-${_jdkver}-openjdk/bin:$PATH"
  export MAVEN_REPOSITORY=${srcdir}/.m2/repository

  ./installers.cmd -Dintellij.build.use.compiled.classes=false -Dintellij.build.target.os=linux -Dbuild.number="${_build}" \
    -Dintellij.build.target.arch=${_arch} -Dintellij.build.linux_tar_gz_without_jre=true
  tar -xf out/idea-ce/artifacts/ideaIC-${_build}-no-jbr${_suffix}.tar.gz -C "${srcdir}"
}

package() {
  cd idea-IC-${_build}

  install -dm 755 "${pkgdir}"/usr/share/{licenses,pixmaps,idea,icons/hicolor/scalable/apps}
  cp -dr --no-preserve='ownership' bin lib plugins "${pkgdir}"/usr/share/idea/
  cp -dr --no-preserve='ownership' license "${pkgdir}"/usr/share/licenses/idea
  ln -s /usr/share/idea/bin/idea.png "${pkgdir}"/usr/share/pixmaps/
  ln -s /usr/share/idea/bin/idea.svg "${pkgdir}"/usr/share/icons/hicolor/scalable/apps/
  install -Dm 644 ../idea.desktop -t "${pkgdir}"/usr/share/applications/
  install -Dm 755 ../idea.sh "${pkgdir}"/usr/bin/idea
  install -Dm 644 build.txt -t "${pkgdir}"/usr/share/idea
  install -Dm 644 product-info.json -t "${pkgdir}"/usr/share/idea

  # riscv64 missing parts
  install -Dm 755 ../pty4j/native/libpty.so -t "${pkgdir}"/usr/share/idea/lib/pty4j/linux/${_alt_arch}
  install -Dm 755 ../intellij-community/platform/sqlite/target/sqlite/linux-${CARCH}/libsqliteij.so \
    -t "${pkgdir}"/usr/share/idea/lib/native/linux-${CARCH}/
}

# vim: ts=2 sw=2 et:
