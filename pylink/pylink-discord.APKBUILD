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
source=""
builddir="$srcdir/$_pyname-$pkgver"

giturl="https://github.com/PyLink/$_pyname.git"
# 2020/15/05 Drop IRC<->Discord formatting conversion, it's horribly buggy
reporev="aea26be2c7834a092f198dda53f58f77e7e95f22"

# There is no test
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
	install -m 0755 -d "$pkgdir$dest/pylinkirc/protocols"
	install -m 0644 -t "$pkgdir$dest/pylinkirc/protocols" protocols/*
}

