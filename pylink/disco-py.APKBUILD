# Contributor: Celelibi <celelibi@gmail.com>
# Maintainer: Celelibi <celelibi@gmail.com>
_pyname="disco-py"
pkgname="py3-$_pyname"
pkgver="0.0.12"
pkgrel=0
pkgdesc="A Python library for Discord"
url="https://github.com/b1naryth1ef/disco"
arch="noarch"
license="MIT"
depends="python3 py3-gevent py3-holster py3-requests py3-six py3-websocket-client"
makedepends="py3-setuptools"
install=""
subpackages=""
source=""
builddir="$srcdir/$_pyname-$pkgver"

giturl="https://github.com/b1naryth1ef/disco.git"
# 2020-03-26: Updated gevent to 1.4.0
reporev="6260ec74a9408990b1193a6132d7ff62bbd69b9a"

# No bdist_wheel command available for setup.py
options="!check"


snapshot() {
	gitdir="$_pyname-$pkgver"
	format="tar.gz"

	git --git-dir "$gitdir" clone --bare $giturl "$gitdir"

	date=$(git --git-dir "$gitdir" log -n 1 --format='%cd' --date=format:'%Y%m%d' $reporev)
	revname=$pkgver"_git$date"
	archivepath="$_pyname-$revname.$format"

	git --git-dir "$gitdir" archive --format="$format" -o "$archivepath" --prefix="$_pyname-$revname/" $reporev

	# Patch the APKBUILD with the right version and source file
	sed -i -e "s/^pkgver=.*/pkgver=\"$revname\"/" APKBUILD
	sed -i -e "s/^source=.*/source=\"$source $archivepath\"/" APKBUILD
}

build() {
	python3 setup.py build
}

check() {
	python3 setup.py test
}

package() {
	python3 setup.py install --prefix=/usr --root="$pkgdir"
}

