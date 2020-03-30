# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="PyYAML"
pkgname="py3-pyyaml"
pkgver="5.3.1"
pkgrel=0
pkgdesc="YAML parser and emitter for Python"
url="https://pypi.org/project/PyYAML/"
arch="x86_64"
license="MIT"
depends="python3 yaml-dev"
makedepends="py3-setuptools build-base python3-dev"
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

