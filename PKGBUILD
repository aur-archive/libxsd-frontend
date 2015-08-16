# Maintainer: kevku <kevku@gmx.com>
pkgname=libxsd-frontend
pkgver=2.0.0
pkgrel=1
pkgdesc="A compiler frontend for the W3C XML Schema definition language"
arch=('x86_64' 'i686')
depends=('xerces-c' 'libcutl')
makedepends=('build')
url="http://www.codesynthesis.com/projects/libxsd-frontend"
license=('GPL2')
source=("http://www.codesynthesis.com/download/$pkgname/2.0/$pkgname-$pkgver.tar.bz2"
	"disable_tests.patch")
md5sums=('040b40977ac8294e8591affe6061f68d'
         '6b3745ec62a6b9367ed7151d0f3cd653')

build() {
  cd "$srcdir/$pkgname-$pkgver"
    patch -N -i "$srcdir/disable_tests.patch"
  #configuration
  mkdir -p \
	build/{ld,cxx/gnu} \
	build/import/lib{cutl,xerces-c}

  cat > build/cxx/configuration-dynamic.make <<- EOF
	cxx_id       := gnu
	cxx_optimize := n
	cxx_debug    := n
	cxx_rpath    := n
	cxx_pp_extra_options :=
	cxx_extra_options    := ${CXXFLAGS}
	cxx_ld_extra_options := ${LDFLAGS}
	cxx_extra_libs       :=
	cxx_extra_lib_paths  :=
		EOF

  cat > build/cxx/gnu/configuration-dynamic.make <<- EOF
	cxx_gnu := g++
	cxx_gnu_libraries :=
	cxx_gnu_optimization_options :=
		EOF
  cat > build/import/libcutl/configuration-dynamic.make <<- EOF
	libcutl_installed := y
		EOF
  cat > build/ld/configuration-lib-dynamic.make <<- EOF
	ld_lib_type   := shared
		EOF
  cat > build/import/libxerces-c/configuration-dynamic.make <<- EOF
	libxerces_c_installed := y
		EOF

  make
}

package() {
 cd "$srcdir/$pkgname-$pkgver"
  install -D xsd-frontend/libxsd-frontend.so $pkgdir/usr/lib/libxsd-frontend.so
  find xsd-frontend -iname "*.cxx" \
		-o -iname "makefile" \
		-o -iname ".git*" \
		-o -iname "*.o" -o -iname "*.d" \
		-o -iname "*.m4" -o -iname "*.l" \
		-o -iname "*.cpp-options" -o -iname "*.so" | xargs rm -f
  
  mkdir $pkgdir/usr/include
  cp -r xsd-frontend $pkgdir/usr/include
}
