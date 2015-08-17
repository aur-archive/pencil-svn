# Contributor: N3ON <archlinux@alunamation.com>
# Contributor: Emiliano Vavassori <syntaxerrormmm@gmail.com>

pkgname=("pencil-svn")
pkgver=382
pkgrel=2
pkgdesc="Animation/drawing software, that lets you create traditional hand-drawn animation using both bitmap and vector graphics"
url="http://pencil-animation.org/"
license=("GPL2")
arch=("i686" "x86_64")
depends=("qt4" "phonon" "ffmpeg")
makedepends=("subversion" "mesa")
conflicts=("pencil")
source=("svn+https://pencil-planner.svn.sourceforge.net/svnroot/pencil-planner/branches/v0.5/" \
	"pencil.desktop" \
	"pencil.sh")
md5sums=("SKIP" \
	"01a9978f3326c5dc0abe946a00e23fca" \
	"67292fa3b8775b3be75e57fafdc699ab")

pkgver() {
	cd "$SRCDEST/v0.5"
	svnversion | tr -d [A-z]
}

build() {
	cd "v0.5"

	# Compile fixs
	sed -e "18d" -e "26d" -i src/main.cpp
	cat <<-EOF | sed --file=- -i src/structure/layersound.h
		s|^#include <phonon>$|#include <phonon/MediaObject>\n\
		#include <phonon/AudioOutput>|
	EOF

	qmake-qt4 \
		"QMAKE_CFLAGS+=-fpermissive" \
		"QMAKE_CXXFLAGS+=-fpermissive"
	make
}

package() {
	cd "v0.5"
	install -d "${pkgdir}/opt/pencil/plugins"
	install -m755 release/plugins/* "${pkgdir}/opt/pencil/plugins"
	install -D -m755 release/Pencil "${pkgdir}/opt/pencil/Pencil"
	install -D -m644 icons/icon.png "${pkgdir}/usr/share/pixmaps/pencil.png"
	install -D -m755 "${srcdir}/pencil.sh" "${pkgdir}/usr/bin/pencil"
	install -D -m644 "${srcdir}/pencil.desktop" \
		"${pkgdir}/usr/share/applications/pencil.desktop"
}

# vim: set noet ff=unix:
