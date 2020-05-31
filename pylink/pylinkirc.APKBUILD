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
source=""
builddir="$srcdir/$_pyname-$pkgver"

giturl="https://github.com/jlu5/$_ghname.git"
# 2020/05/30 Implement path configuration for files created by pylink
reporev="9470e9329a0231d93ac2add8ba83027ce1fc7ffa"

# Tests fail to import pylinkirc
options="!check"

snapshot() {
	gitdir="$_pyname-$pkgver"
	format="tar.gz"

	git --git-dir "$gitdir" clone --bare $giturl "$gitdir"

	date=$(git --git-dir "$gitdir" log -n 1 --format='%cd' --date=format:'%Y%m%d' $reporev)
	revname=$pkgver"_git$date"
	archivepath="$_pyname-$revname.$format"

	git --git-dir "$gitdir" archive --format="$format" -o "$archivepath" --prefix="$_pyname-$revname/" $reporev
	echo "Created archive $(pwd)/$archivepath"

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

