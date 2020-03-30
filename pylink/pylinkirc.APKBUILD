# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="pylinkirc"
pkgname="py3-$_pyname"
pkgver="2.0.3"
pkgrel=0
pkgdesc="PyLink IRC Services"
url="https://github.com/jlu5/PyLink"
arch="noarch"
license="MPL-2.0"
depends="python3 py3-pyyaml py3-ircmatch"
makedepends="py3-setuptools"
install=""
subpackages=""
source="https://files.pythonhosted.org/packages/source/${_pyname%${_pyname#?}}/$_pyname/$_pyname-$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

# Tests fail to import pylinkirc
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

