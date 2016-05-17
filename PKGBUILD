# Maintainer: Michael Hansen <zrax0111 gmail com>

pkgname=visual-studio-code-oss
pkgdesc='Visual Studio Code for Linux, Open Source version'
pkgver=1.1.1
pkgrel=1
arch=('i686' 'x86_64')
url='https://code.visualstudio.com/'
license=('MIT')
makedepends=('npm' 'gulp' 'python2')
depends=('gtk2' 'gconf')
conflicts=('vscode-oss')
provides=('vscode-oss')

source=("https://github.com/Microsoft/vscode/archive/${pkgver}.tar.gz"
        "${pkgname}.desktop"
        'product_json.patch')
sha1sums=('71547fefd1eaf174ca7484c668f2186d906cfb67'
          '9c4176c4d99103736df6746ca174b5026bd57e6b'
          'a944f430c33b425f52752f6a0feae0b0f4abfb29')

case "$CARCH" in
    i686)
        _vscode_arch=ia32
        ;;
    x86_64)
        _vscode_arch=x64
        ;;
    *)
        # Needed for mksrcinfo
        _vscode_arch=DUMMY
        ;;
esac

prepare() {
    cd "${srcdir}/vscode-${pkgver}"

    patch -p1 -i "${srcdir}/product_json.patch"
}

build() {
    cd "${srcdir}/vscode-${pkgver}"

    ./scripts/npm.sh install

    # The default memory limit is too low on some systems (i686?). This will
    # set it to 2GB -- change it if this number doesn't work for your system
    node --max_old_space_size=2048 /usr/bin/gulp vscode-linux-${_vscode_arch}
}

package() {
    install -m 0755 -d "${pkgdir}/opt/VSCode-OSS"
    cp -r "${srcdir}/VSCode-linux-${_vscode_arch}"/* "${pkgdir}/opt/VSCode-OSS"

    # Include symlink in system bin directory
    install -m 0755 -d "${pkgdir}/usr/bin"
    ln -s '/opt/VSCode-OSS/code-oss' "${pkgdir}/usr/bin/${pkgname}"

    # Add .desktop file
    install -D -m644 "${srcdir}/${pkgname}.desktop" \
            "${pkgdir}/usr/share/applications/${pkgname}.desktop"

    # Install license file
    install -D -m644 "${srcdir}/VSCode-linux-${_vscode_arch}/resources/app/LICENSE.txt" \
            "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
