# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>
# Contributor: James Rayner <james@archlinux.org>
# Contributor: Partha Chowdhury <kira.laucas@gmail.com>

_pkgname=conky
pkgname=conky-git
pkgver=1.999.425.g689b404
pkgrel=1
pkgdesc='Lightweight system monitor for X'
url='http://conky.sourceforge.net/'
license=('BSD' 'GPL')
arch=('i686' 'x86_64')
makedepends=('pkg-config' 'cmake' 'docbook2x' 'git')
depends=('glib2' 'curl' 'lua' 'wireless_tools' 'libxml2' 'libxft' 'libxdamage' 'imlib2')
conflicts=('conky')
provides=('conky')
source=('git://github.com/brndnmtthws/conky'
        'conky_doc.patch'
        'conky_cmake.patch')
md5sums=('SKIP'
         'dea392ef33e6c4cd10b0ef807917b404'
         'e060d262d4d6bf7b723fc2d90cfa6062')

pkgver() {
	cd $_pkgname
	git describe --always | sed -e 's|-|.|g'
}

build() {
	cd "$srcdir/$_pkgname"

	patch -Np1 -i $srcdir/conky_doc.patch
	patch -Np1 -i $srcdir/conky_cmake.patch

	cmake \
	-D CMAKE_INSTALL_PREFIX:PATH=/usr \
	-D CMAKE_BUILD_TYPE:STRING="Release" \
	-D MAINTAINER_MODE:BOOL=ON \
	-D BUILD_IBM:BOOL=ON \
	-D BUILD_CURL:BOOL=ON \
	-D BUILD_RSS:BOOL=ON \
	-D BUILD_WEATHER_METAR:BOOL=ON \
	-D BUILD_WEATHER_XOAP:BOOL=ON \
	-D BUILD_IMLIB2:BOOL=ON \
	-D BUILD_WLAN:BOOL=ON \
	../conky

	# Uncomment to change configuration settings interactively.
	# ccmake .

	make
}

package() {
	cd "$srcdir/$_pkgname"
	make DESTDIR="$pkgdir" install
	install -d "$pkgdir/usr/share/licenses/$_pkgname"
	install -m644 ../conky/{COPYING,LICENSE}* "$pkgdir/usr/share/licenses/$_pkgname"
	install -Dm644 extras/vim/syntax/conkyrc.vim "$pkgdir"/usr/share/vim/vimfiles/syntax/conkyrc.vim
	install -Dm644 extras/vim/ftdetect/conkyrc.vim "$pkgdir"/usr/share/vim/vimfiles/ftdetect/conkyrc.vim
	install -d "$pkgdir/etc/$_pkgname"
	install -Dm644 "$pkgdir"/usr/share/doc/conky-2.0.0_pre/conky{,_no_x11}.conf "$pkgdir"/etc/conky
}
