# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="expiringdict"
pkgname="py3-$_pyname"
pkgver="1.2.0"
pkgrel=0
pkgdesc="Dictionary with auto-expiring values for caching purposes"
url="https://www.mailgun.com/"
arch="noarch"
license="Apache-2.0"
depends="python3"
makedepends="py3-setuptools"
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

