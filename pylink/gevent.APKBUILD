# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="gevent"
pkgname="py3-$_pyname"
pkgver="1.5.0"
pkgrel=0
pkgdesc="Coroutine-based network library"
url="http://www.gevent.org/"
arch="x86_64"
license="MIT"
depends="python3 py3-greenlet"
makedepends="py3-setuptools build-base python3-dev"
install=""
subpackages=""
source="https://files.pythonhosted.org/packages/source/${_pyname%${_pyname#?}}/$_pyname/$_pyname-$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

# No bdist_wheel command available for setup.py
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

