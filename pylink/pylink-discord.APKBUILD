# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="pylink-discord"
pkgname="py3-$_pyname"
pkgver="0.2.0"
pkgrel=0
pkgdesc="Discord integration support for PyLink"
url="https://github.com/PyLink/pylink-discord"
arch="noarch"
license="GPL-2.0-only"
depends="python3 py3-pylinkirc py3-gevent py3-disco-py"
makedepends="py3-setuptools"
install=""
subpackages=""
source="$_pyname-$pkgver.tar.gz::https://github.com/PyLink/pylink-discord/archive/$pkgver.tar.gz"
builddir="$srcdir/$_pyname-$pkgver"

# There is no test
options="!check"

build() {
	true
}

check() {
	false
}

package() {
	cd "$builddir"
	dest=$(python3 -c 'import sysconfig; print(sysconfig.get_paths()["purelib"])')
	install -m 0755 -d "$pkgdir/usr/bin"
	install -m 0755 -t "$pkgdir/usr/bin" pylink-discord
	install -m 0755 -d "$pkgdir$dest/$_pyname/protocols"
	install -m 0644 -t "$pkgdir$dest/$_pyname/protocols" protocols/*
	echo "############################################################"
	echo "############################################################"
	echo "############################################################"
	echo "Don't forget to add $dest/$_pyname/protocols to your pylink.yml config file"
	echo "in the option pylink/protocol_dirs."
	echo "############################################################"
	echo "############################################################"
	echo "############################################################"
}

