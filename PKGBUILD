pkgname=rocprofiler-register
pkgver=7.0.1
pkgrel=2
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
sha256sums=(f4825d556b5223633fb14a3dfe91c2afe1bc2b2c89598135e74afb4199af46b4)

prepare() {
    cd ${pkgname}-rocm-${pkgver}

    # Remove cpack packaging
    sed -i '116d' CMakeLists.txt

    # Do not hardcode install lib
    sed -i 's@set(CMAKE_INSTALL_LIBDIR@#set(CMAKE_INSTALL_LIBDIR@' CMakeLists.txt

    # find_package() calls on global scope
    sed -i 's/add_subdirectory(external)/find_package(fmt REQUIRED)\nfind_package(glog REQUIRED)/' CMakeLists.txt
}

build() {
    cd ${pkgname}-rocm-${pkgver}

    local cmake_args=(
        -B flarebird-build
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
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
