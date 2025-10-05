pkgname=rocprofiler-register
pkgver=6.4.4
pkgrel=1
pkgdesc="Helper library for the ROCprofiler (v2) library"
arch=('x86_64')
url="https://github.com/ROCm/rocprofiler-register"
license=('MIT')
depends=(
    'rocm-core'
    'glibc'
    'gcc-libs'
    'fmt'
    'google-glog'
)
makedepends=(
    'cmake'
    'rocm-cmake'
)
source=(https://github.com/ROCm/rocprofiler-register/archive/rocm-${pkgver}/${pkgname}-rocm-${pkgver}.tar.gz)
sha256sums=(63e14e7b03c517c594dbc1ce0ee5c632b29449820d54a26de844e6126f074db1)

prepare() {
    cd ${pkgname}-rocm-${pkgver}

    # Remove cpack packaging
    sed -i '116d' CMakeLists.txt

    # find_package() calls on global scope
    sed -i 's/add_subdirectory(external)/find_package(fmt REQUIRED)\nfind_package(glog REQUIRED)/' CMakeLists.txt

}

build() {
    cd ${pkgname}-rocm-${pkgver}

    local cmake_args=(
        -B flarebird-build
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr/lib64/rocm
        -D CMAKE_INSTALL_LIBDIR=lib64
        -D ROCPROFILER_REGISTER_BUILD_GLOG=OFF
        -D ROCPROFILER_REGISTER_BUILD_FMT=OFF
        -W no-dev
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package() {
    cd ${pkgname}-rocm-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build
}
