# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="ircmatch"
pkgname="py3-$_pyname"
pkgver="1.2"
pkgrel=0
pkgdesc="Library for matching IRC masks based on Atheme"
url="http://kaniini.dereferenced.org/projects/ircmatch.html"
arch="x86_64"
license="UNKNOWN"
depends="python3"
makedepends="py3-setuptools build-base python3-dev"
install=""
subpackages=""
source="https://files.pythonhosted.org/packages/source/${_pyname%${_pyname#?}}/$_pyname/$_pyname-$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

# No check implemented
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

