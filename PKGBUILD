#_                   _ _ _  _ _____ _  _
#| | _______   ____ _| | | || |___  | || |
#| |/ / _ \ \ / / _` | | | || |_ / /| || |_
#|   <  __/\ V / (_| | | |__   _/ / |__   _|
#|_|\_\___| \_/ \__,_|_|_|  |_|/_/     |_|

#Maintainer: kevall474 <kevall474@tuta.io> <https://github.com/kevall474>
#Credits: Laurent Carlier <lordheavym@archlinux.org>
#Credits: Cyano Hao <c@cyano.cn>

pkgname=directx-headers-git
pkgdesc="DirectX headers for using D3D12"
pkgver=1.602.0.r17.g19ec5d3
pkgrel=1
arch=(x86_64)
url='https://github.com/microsoft/DirectX-Headers'
license=(MIT)
makedepends=(meson git ninja)
conflicts=(directx-headers)
provides=(directx-headers directx-headers-git)
source=(git+https://github.com/microsoft/DirectX-Headers.git)
md5sums=(SKIP)


pkgver(){
  cd ${srcdir}/DirectX-Headers/
  # cutting off 'foo-' prefix that presents in the git tag
  git describe --long --tags --exclude *preview --exclude *mesa* --exclude *-r1| sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build(){
# LTO breaks mesa...
export CXXFLAGS="$CXXFLAGS -fno-lto"

  cd ${srcdir}/DirectX-Headers/

  rm -rf build

  mkdir -p -v build

  meson setup build/ \
  -D b_ndebug=true \
  -D buildtype=plain \
  --wrap-mode=nofallback \
  -Dprefix=/usr \
  -D sysconfdir=/etc \
  -Dbuild-test=false

  meson configure build/

  meson compile -C build/

  ninja -C build/
}

package(){
  DESTDIR="$pkgdir" ninja -C ${srcdir}/DirectX-Headers/build/ install

  # install licence
  install -Dm644 "${srcdir}"/DirectX-Headers/LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
