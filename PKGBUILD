# Maintainer: Barna Csorogi <barna.csorogi@gmail.com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Robert Hollencamp <rhollencamp@gmail.com>
pkgname=raknet
pkgver=4.082
pkgrel=1
pkgdesc="Cross platform C++ network library"
arch=('i686' 'x86_64')
url="http://www.jenkinssoftware.com/"
license=('custom')
depends=('gcc-libs')
makedepends=('cmake')
source=(http://www.jenkinssoftware.com/raknet/downloads/RakNet_PC-${pkgver}.zip
        license.txt)
md5sums=('6de6830c3b39f9d8ec990e8c984a29b5'
         '6bd636fe028aac184c20b3a50d83cd79')

build()
{
  cd ${srcdir}
  mkdir -p build
  cd build

  # enable LIBCAT_SECURITY features
  sed -i -e s@//#define@#define@g ../Source/NativeFeatureIncludesOverrides.h

  # add missing header to sources
  sed -i '23i#include <netdb.h>' ../Source/TCPInterface.cpp
  sed -i '1i#include <netdb.h>' ../Source/RakNetSocket2_Berkley_NativeClient.cpp
  sed -i '41i#include <netdb.h>' ../Source/SocketLayer.cpp
  sed -i '1i#include <netdb.h>' ../Source/RakNetSocket2_Windows_Linux.cpp

  cmake -DCMAKE_BUILD_TYPE=Release       \
        -DCMAKE_INSTALL_PREFIX=/usr      \
        -DRAKNET_GENERATE_SAMPLES=False  ..

  make
}

package()
{
  cd ${pkgdir}
  
  # DESTDIR is not used correctly so copy library and its headers manually
  mkdir -p usr/share/licenses/${pkgname}
  cp ${srcdir}/license.txt usr/share/licenses/${pkgname}/license.txt
  
  mkdir -p usr/lib/
  cp ${srcdir}/build/Lib/DLL/libRakNetDLL.so      usr/lib/
  cp ${srcdir}/build/Lib/LibStatic/libRakNetLibStatic.a  usr/lib/
  
  mkdir -p usr/include/raknet/
  install -m644 ${srcdir}/Source/*.h usr/include/raknet/
}
