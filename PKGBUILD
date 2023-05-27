pkgname=x265-aMod
pkgver=3.5.r103.g34532bda1
pkgrel=3
pkgdesc='Open source H.265/HEVC video encoder (aMod version)'
arch=('x86_64')
url='https://github.com/plantysnake/x265-aMod'
license=('GPL')
depends=('gcc-libs')
makedepends=('git' 'cmake' 'nasm')
provides=('x265' 'libx265.so')
source=('git+https://github.com/plantysnake/x265-aMod.git')
sha256sums=('SKIP')

pkgver() {
    git -C x265-aMod describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//'
}

prepare() {
  sed -i '/set(X265_BUILD 2[0-9][0-9])/cset(X265_BUILD 199)' x265-aMod/source/CMakeLists.txt
}

build() {
    export CFLAGS+=' -Wno-unused-parameter -Wno-unused-result' CXXFLAGS+=' -Wno-unused-parameter -Wno-unused-result'
    cmake -S x265-aMod/source -B build-12 \
        -DCMAKE_INSTALL_PREFIX='/usr' \
        -DHIGH_BIT_DEPTH='ON' \
        -DMAIN12='ON' \
        -DEXPORT_C_API='OFF' \
        -DENABLE_CLI='OFF' \
		-DENABLE_LIBNUMA='ON' \
		-DENABLE_PIC='ON' \
        -DENABLE_SHARED='OFF' \
        -DCMAKE_ASM_NASM_FLAGS=-w-macro-params-legacy \
        -Wno-dev
    make -C build-12
    
    cmake -S x265-aMod/source -B build-10 \
        -DCMAKE_INSTALL_PREFIX='/usr' \
        -DHIGH_BIT_DEPTH='ON' \
        -DEXPORT_C_API='OFF' \
        -DENABLE_CLI='OFF' \
		-DENABLE_LIBNUMA='ON' \
		-DENABLE_PIC='ON' \
        -DENABLE_SHARED='OFF' \
        -DCMAKE_ASM_NASM_FLAGS=-w-macro-params-legacy \
        -Wno-dev
    make -C build-10
    
    cmake -S x265-aMod/source -B build \
        -DCMAKE_INSTALL_PREFIX:PATH='/usr' \
        -DENABLE_SHARED='ON' \
        -DENABLE_HDR10_PLUS='ON' \
        -DEXTRA_LIB='x265_main10.a;x265_main12.a' \
        -DEXTRA_LINK_FLAGS='-L.' \
        -DENABLE_CLI='ON' \
		-DENABLE_LIBNUMA='ON' \
		-DENABLE_PIC='ON' \
        -DLINKED_10BIT='ON' \
        -DLINKED_12BIT='ON' \
        -DCMAKE_ASM_NASM_FLAGS=-w-macro-params-legacy \
        -Wno-dev
    ln -s ../build-10/libx265.a build/libx265_main10.a
    ln -s ../build-12/libx265.a build/libx265_main12.a
    make -C build
}

package() {
    make -C build DESTDIR="$pkgdir" install
}
