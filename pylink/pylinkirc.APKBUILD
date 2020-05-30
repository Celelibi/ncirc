# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="pylinkirc"
_ghname="PyLink"
_ghver="3.0.0"
pkgname="py3-$_pyname"
pkgver=${_ghver/-/_}
pkgrel=0
pkgdesc="PyLink IRC Services"
url="https://github.com/jlu5/PyLink"
arch="noarch"
license="MPL-2.0"
depends="python3 py3-pyyaml py3-ircmatch"
makedepends="py3-setuptools"
install=""
subpackages=""
source="$_ghname-$_ghver.tar.gz::https://github.com/jlu5/$_ghname/archive/$_ghver.tar.gz"
builddir="$srcdir/$_ghname-$_ghver"

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

