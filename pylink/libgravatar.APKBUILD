# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="libgravatar"
pkgname="py3-$_pyname"
pkgver="0.2.3"
pkgrel=0
pkgdesc="A library that provides a Python 3 interface for the Gravatar API"
url="https://github.com/pabluk/libgravatar"
arch="noarch"
license="GPL-3.0"
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

