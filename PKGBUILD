pkgname=akhenaten
pkgver=HEAD
pkgrel=1
pkgdesc="Open-source engine for Pharaoh (+Cleopatra) city-building game"
arch=(x86_64)
url=https://github.com/dalerank/Akhenaten
license=(AGPL3)
depends=(sdl2 sdl2_mixer sdl2_image)
makedepends=(cmake git ninja glm)
optdepends=('libpng: needed for image loading')
provides=(akhenaten-git)
conflicts=(akhenaten)
source=($pkgname::git+$url.git#tag=$pkgver)
sha256sums=(SKIP)

prepare() {
  cd "$srcdir/$pkgname"

  patch -p1 <<'EOF'
diff --git a/src/js/js.cpp b/src/js/js.cpp
index 1234567..89abcde 100644
--- a/src/js/js.cpp
+++ b/src/js/js.cpp
@@
-#elif defined(GAME_PLATFORM_LINUX) || defined(GAME_PLATFORM_MACOSX)
-            realpath(conpath, buffer);
+#elif defined(GAME_PLATFORM_LINUX) || defined(GAME_PLATFORM_MACOSX)
+            char resolved[PATH_MAX];
+            if (realpath(conpath, resolved)) {
+                buffer = resolved;
+            } else {
+                continue;
+            }
EOF
}

build() {
  cd "$srcdir/$pkgname"
  mkdir -p build
  cd build
  cmake -G Ninja \
    -DCMAKE_BUILD_TYPE=None \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_C_FLAGS="-g -O0" \
    -DCMAKE_CXX_FLAGS="-g -O0" \
    ..
  ninja
}

package() {
  cd "$srcdir/$pkgname/build"
  DESTDIR="$pkgdir" ninja install
}
