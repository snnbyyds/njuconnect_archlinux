# Maintainer: snnbyyds <snnbyyds@gmail.com>
pkgname=njuconnect-fork
pkgver=TestBuild15.r8.f940390
pkgrel=1
pkgdesc="NJU VPN protocol reimplementation in Go"
arch=('x86_64' 'aarch64')
url="https://github.com/snnbyyds/NJUConnect"
license=('AGPL-3.0-only')
depends=('glibc')
conflicts=('njuconnect')
makedepends=(
    'git'
    'go'
    'libx11'
    'libxcursor'
    'libxrandr'
    'libxinerama'
    'libxi'
    'libglvnd'
)
_tag="f940390a10cd7bf09bb0351d4fea2acda320e974"
source=($pkgname::git+$url.git#tag=$_tag)
sha256sums=('SKIP')

pkgver() {
    cd "$pkgname"
    printf "%s" "$(git describe --long --tags | sed 's/\([^-]*-\)g/r\1/;s/-/./g')"
}

prepare() {
    cd "$pkgname"
    mkdir -p build/
    cat >build/njuconnect.desktop <<EOF
[Desktop Entry]
Type=Application
Name=NJUConnect
Comment=NJU VPN Client
Exec=njuconnect-gui
Icon=network-vpn
Categories=Network;
Terminal=true
EOF
}

build() {
    cd "$pkgname"
    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"
    go build -o build/njuconnect .
    go build -o build/njuconnect-gui ./gui/
}

package() {
    cd "$pkgname"
    install -Dm755 build/njuconnect "$pkgdir/usr/bin/njuconnect"
    install -Dm755 build/njuconnect-gui "$pkgdir/usr/bin/njuconnect-gui"
    install -Dm644 build/njuconnect.desktop "$pkgdir/usr/share/applications/njuconnect.desktop"
    install -Dm644 -t "$pkgdir/usr/share/doc/$pkgname/" README.md
}
