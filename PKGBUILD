# custom variables
_hkgname=haskell-src-exts
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-haskell-src-exts-legacy
pkgver=1.10.2
pkgrel=2
pkgdesc="Manipulating Haskell source: abstract syntax, lexer, parser, and pretty-printer"
url="http://code.haskell.org/haskell-src-exts"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=('ghc')
depends=("ghc"
         "haskell-cpphs"
         "happy")
provides=('haskell-haskell-src-exts')
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
sha256sums=('34bad3970f3602cfdd0cd0d4a51b5a68abec61b3969632eeb57ff61880cf1026')
install="${pkgname}.install"

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
    rm -f ${pkgdir}/usr/share/doc/ghc/html/libraries/haskell-src-exts
}
