# Maintainer: Tetsumaki <http://goo.gl/YMBdA>

pkgname=ultracopier
pkgver=1.0.1.1
pkgrel=1
pkgdesc="Advanced file copying tool, new version with qt5"
arch=('i686' 'x86_64')
url="http://ultracopier.first-world.info/"
license=('GPL3')
depends=('qt5-base')
makedepends=('qt5-base' 'qt5-tools' 'qt5-qtsystems-git')
conflicts=('ultracopier3')
source=("http://ompldr.org/vaWVqbA/${pkgname}-src-${pkgver}.tar.xz")
sha1sums=('fe17f8cb0880ae6e514d3f7ff0806120c2c40a27')

build() {
	cd "${srcdir}/ultracopier-src"

	case ${CARCH} in
		i686)
			find -name "informations.xml" -exec sed -i -r "s/<architecture>.*<\/architecture>/<architecture>linux-x86-pc<\/architecture>/g" {} \;
			;;
		x86_64)
			find -name "informations.xml" -exec sed -i -r "s/<architecture>.*<\/architecture>/<architecture>linux-x86_64-pc<\/architecture>/g" {} \;
			;;
	esac

	find -name "informations.xml" -exec sed -i -r "s/<version>.*<\/version>/<version>${pkgver}<\/version>/g" {} \;
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_VERSION_PORTABLE/\/\/#define ULTRACOPIER_VERSION_PORTABLE/g" {} \;
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_VERSION_PORTABLEAPPS/\/\/#define ULTRACOPIER_VERSION_PORTABLEAPPS/g" {} \;
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_DEBUG/\/\/#define ULTRACOPIER_DEBUG/g" {} \; > /dev/null 2>&1
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_PLUGIN_DEBUG/\/\/#define ULTRACOPIER_PLUGIN_DEBUG/g" {} \; > /dev/null 2>&1
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_PLUGIN_DEBUG_WINDOW/\/\/#define ULTRACOPIER_PLUGIN_DEBUG_WINDOW/g" {} \; > /dev/null 2>&1
	find -name "Variable.h" -exec sed -i "s/#define ULTRACOPIER_PLUGIN_ALL_IN_ONE/\/\/#define ULTRACOPIER_PLUGIN_ALL_IN_ONE/g" {} \; > /dev/null 2>&1
	find -name "Variable.h" -exec sed -i "s/\/\/#define ULTRACOPIER_VERSION_ULTIMATE/#define ULTRACOPIER_VERSION_ULTIMATE/g" {} \; > /dev/null 2>&1

	lrelease -nounfinished -compress -removeidentical -silent "ultracopier-core.pro"
	qmake "ultracopier-core.pro"
	make

	cd "plugins/CopyEngine/Ultracopier"
	qmake "CopyEngine.pro"
	make

	cd "${srcdir}/ultracopier-src/plugins/Listener/catchcopy-v0002"
	qmake "listener.pro"
	make

	cd "${srcdir}/ultracopier-src/plugins/SessionLoader/KDE4"
	qmake "sessionLoader.pro"
	make

	cd "${srcdir}/ultracopier-src/plugins/Themes/Oxygen"
	qmake "interface.pro"
	make

	cd "${srcdir}/ultracopier-src"
	lrelease -nounfinished -compress -removeidentical -silent "ultracopier-core.pro"
	for project in `find plugins/ plugins-alternative/ -maxdepth 2 -type d`
	do
		if [ -f "${project}/*.pro" ]
		then
			lrelease -nounfinished -compress -removeidentical -silent "${project}/*.pro"
		fi
	done
	find -iname "*.ts" -exec rm {} \;
}

package(){
	cd "${srcdir}/ultracopier-src"

	install -Dm644 "resources/ultracopier.desktop" "${pkgdir}/usr/share/applications/ultracopier.desktop"
	install -Dm644 "resources/ultracopier-128x128.png" "${pkgdir}/usr/share/pixmaps/ultracopier.png"
	install -Dm755 "ultracopier" "${pkgdir}/usr/bin/ultracopier"

	install -Dm644 "plugins/CopyEngine/Ultracopier/informations.xml" "${pkgdir}/usr/share/ultracopier/CopyEngine/Ultracopier/informations.xml"
	install -Dm755 "plugins/CopyEngine/Ultracopier/libcopyEngine.so" "${pkgdir}/usr/share/ultracopier/CopyEngine/Ultracopier/libcopyEngine.so"

	install -Dm644 "plugins/Listener/catchcopy-v0002/informations.xml" "${pkgdir}/usr/share/ultracopier/Listener/catchcopy-v0002/informations.xml"
	install -Dm755 "plugins/Listener/catchcopy-v0002/liblistener.so" "${pkgdir}/usr/share/ultracopier/Listener/catchcopy-v0002/liblistener.so"

	install -Dm644 "plugins/SessionLoader/KDE4/informations.xml" "${pkgdir}/usr/share/ultracopier/SessionLoader/KDE4/informations.xml"
	install -Dm755 "plugins/SessionLoader/KDE4/libsessionLoader.so" "${pkgdir}/usr/share/ultracopier/SessionLoader/KDE4/libsessionLoader.so"

	install -Dm644 "plugins/Themes/Oxygen/informations.xml" "${pkgdir}/usr/share/ultracopier/Themes/Oxygen/informations.xml"
	install -Dm755 "plugins/Themes/Oxygen/libinterface.so" "${pkgdir}/usr/share/ultracopier/Themes/Oxygen/libinterface.so"

	for Z in ar de el es fr hi id it ja ko nl no pl pt ru th tr zh; do
		install -Dm644 "plugins/Languages/${Z}/flag.png" "${pkgdir}/usr/share/ultracopier/Languages/${Z}/flag.png"
		install -Dm644 "plugins/Languages/${Z}/informations.xml" "${pkgdir}/usr/share/ultracopier/Languages/${Z}/informations.xml"
		install -Dm644 "plugins/Languages/${Z}/translation.qm" "${pkgdir}/usr/share/ultracopier/Languages/${Z}/translation.qm"
		install -Dm644 "plugins/CopyEngine/Ultracopier/Languages/${Z}/translation.qm" "${pkgdir}/usr/share/ultracopier/CopyEngine/Ultracopier/Languages/${Z}/translation.qm"
		install -Dm644 "plugins/Themes/Oxygen/Languages/${Z}/translation.qm" "${pkgdir}/usr/share/ultracopier/Themes/Oxygen/Languages/${Z}/translation.qm"
	done
}
