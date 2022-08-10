#https://github.com/calamares/calamares/releases

pkgname=ramallahos-calamares
_pkgname=calamares
pkgver=3.2.60
pkgrel=01
pkgdesc='Distribution-independent installer framework'
arch=('i686' 'x86_64')
license=(GPL)
url="https://github.com/calamares/calamares/releases"
license=('LGPL')
depends=('kconfig' 'kcoreaddons' 'kiconthemes' 'ki18n' 'kio' 'solid' 'yaml-cpp' 'kpmcore>=4.2.0' 'mkinitcpio-openswap'
         'boost-libs' 'ckbcomp' 'hwinfo' 'qt5-svg' 'polkit-qt5' 'gtk-update-icon-cache' 'plasma-framework'
         'qt5-xmlpatterns' 'squashfs-tools' 'libpwquality' 'efibootmgr' 'icu')
conflicts=('arco-calamares' 'arco-calamares-comp' 'arco-calamares-dev')
makedepends=('extra-cmake-modules' 'qt5-tools' 'qt5-translations' 'git' 'boost')
backup=('usr/share/calamares/modules/bootloader.conf'
        'usr/share/calamares/modules/displaymanager.conf'
        'usr/share/calamares/modules/initcpio.conf'
        'usr/share/calamares/modules/unpackfs.conf')

source=("$_pkgname-$pkgver::$url/download/v$pkgver/$_pkgname-$pkgver.tar.gz"
	"ucode_main.py"
	"ucode_module.desc"
	"dm_main.py"
	"packages_main.py"
	"cal-ramallahos.desktop"
	"cal-ramallahos-debugging.desktop"
	"calamares_polkit")

sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')


prepare() {
	sed -i -e 's/"Install configuration files" OFF/"Install configuration files" ON/' "$srcdir/${_pkgname}-${pkgver}/CMakeLists.txt"
	sed -i -e 's/# DEBUG_FILESYSTEMS/DEBUG_FILESYSTEMS/' "$srcdir/${_pkgname}-${pkgver}/CMakeLists.txt"
	sed -i -e "s/desired_size = 512 \* 1024 \* 1024  \# 512MiB/desired_size = 512 \* 1024 \* 1024 \* 4  \# 2048MiB/" "$srcdir/${_pkgname}-${pkgver}/src/modules/fstab/main.py"
	cd ${_pkgname}-${pkgver}
	_patchver="$(cat CMakeLists.txt | grep -m3 -e CALAMARES_VERSION_PATCH | grep -o "[[:digit:]]*" | xargs)"
	sed -i -e "s|CALAMARES_VERSION_PATCH $_patchver|CALAMARES_VERSION_PATCH $_patchver-$pkgrel|g" CMakeLists.txt
	install -Dm644 "${srcdir}/ucode_main.py" "${srcdir}/$_pkgname-$pkgver/src/modules/ucode/main.py"
	install -Dm644 "${srcdir}/ucode_module.desc" "${srcdir}/$_pkgname-$pkgver/src/modules/ucode/module.desc"
	install -Dm644 "${srcdir}/dm_main.py" "${srcdir}/$_pkgname-$pkgver/src/modules/displaymanager/main.py"
	install -Dm644 "${srcdir}/packages_main.py" "${srcdir}/$_pkgname-$pkgver/src/modules/packages/main.py"
}

build() {
	cd $_pkgname-$pkgver
	mkdir -p build
	cd build
	cmake .. \
	-DCMAKE_BUILD_TYPE=Release \
	-DCMAKE_INSTALL_PREFIX=/usr \
	-DCMAKE_INSTALL_LIBDIR=lib \
	-DWITH_PYTHONQT=OFF \
	-DWITH_KF5DBus=OFF \
	-DBoost_NO_BOOST_CMAKE=ON \
	-DWEBVIEW_FORCE_WEBKIT=OFF \
	-DSKIP_MODULES="webview tracking interactiveterminal initramfs \
	initramfscfg dracut dracutlukscfg \
	dummyprocess dummypython dummycpp \
	dummypythonqt services-openrc \
	keyboardq localeq welcomeq"
	make
}

package() {
	cd ${_pkgname}-${pkgver}/build
	make DESTDIR="$pkgdir" install
	install -Dm644 "$srcdir/cal-ramallahos.desktop" "$pkgdir/usr/share/applications/cal-ramallahos.desktop"
	install -Dm644 "$srcdir/cal-ramallahos-debugging.desktop" "$pkgdir/usr/share/applications/cal-ramallahos-debugging.desktop"
	install -Dm755 "$srcdir/calamares_polkit" "$pkgdir/usr/bin/calamares_polkit"
	rm "$pkgdir/usr/share/applications/calamares.desktop"
}














