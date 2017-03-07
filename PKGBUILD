# Maintainer: Jason McNeil <jason@jasonrm.net>

pkgname=nfs-ganesha
pkgver=2.5.dev.16.r0.g7808ba522
pkgrel=1
pkgdesc='NFS-Ganesha is an NFSv3,v4,v4.1 fileserver that runs in user mode on most UNIX/Linux systems'
arch=('x86_64')
url="https://github.com/nfs-ganesha/nfs-ganesha"
license=('LGPL')
depends=('libgssglue' 'jemalloc' 'nfsidmap' 'nfs-utils' 'nss')
makedepends=('cmake' 'git' 'zfs' 'glusterfs' 'ceph')
optdepends=('ceph')
provides=()
conflicts=()
backup=('etc/ganesha/ganesha.conf')
options=()
install=
changelog=
source=('git+https://github.com/nfs-ganesha/nfs-ganesha.git')
sha256sums=('SKIP')

prepare() {
    cd "${srcdir}"/"$pkgname"
    git checkout "V${pkgver}"
    git submodule update --init
    sed 's/lib64/lib/g' -i src/CMakeLists.txt
}

build() {
    cd "${srcdir}"/"$pkgname"
    test -d build && rm -rf build
    mkdir -p build

    cd build
    # list of options defaults: grep ^option CMakeLists.txt
    cmake \
        -DUSE_DBUS=OFF \
        -DCMAKE_BUILD_TYPE=Release \
        -DUSE_FSAL_RGW=ON \
        ../src
    make
}

package() {
    cd "${srcdir}"/"$pkgname"/build

    make DESTDIR="${pkgdir}" install

    cd "${pkgdir}"

    mv var/run run
    rmdir var

    install -d -m 755 usr/lib/systemd/system/
    install -D -m 644 "${srcdir}"/"${pkgname}"/src/scripts/systemd/{nfs-ganesha,nfs-ganesha-lock,nfs-ganesha-config}.service usr/lib/systemd/system/
}
