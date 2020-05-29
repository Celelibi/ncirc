# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="holster"
pkgname="py3-$_pyname"
pkgver="2.0.0"
pkgrel=0
pkgdesc="A set of Python utilities"
url="https://github.com/b1naryth1ef/holster"
arch="noarch"
license="Apache-2.0"
depends="python3"
makedepends="py3-setuptools"
install=""
subpackages=""
source="https://files.pythonhosted.org/packages/source/${_pyname%${_pyname#?}}/$_pyname/$_pyname-$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

# Tests not working from the build directory
options="!check"

build() {
	python3 setup.py build
}

check() {
	python3 setup.py test
}

package() {
	python3 setup.py install --prefix=/usr --root="$pkgdir"
}

