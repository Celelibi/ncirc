# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="passlib"
pkgname="py3-$_pyname"
pkgver="1.7.2"
pkgrel=0
pkgdesc="Comprehensive password hashing framework supporting over 30 schemes"
url="https://bitbucket.org/ecollins/passlib"
arch="noarch"
license="BSD"
depends="python3"
makedepends="py3-setuptools"
install=""
subpackages=""
source="https://files.pythonhosted.org/packages/source/${_pyname%${_pyname#?}}/$_pyname/$_pyname-$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

build() {
	python3 setup.py build
}

check() {
	python3 setup.py test
}

package() {
	python3 setup.py install --prefix=/usr --root="$pkgdir"
}

