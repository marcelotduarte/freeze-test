# Maintainer: Marcelo Duarte https://github.com/marcelotduarte

_realname=cx-freeze
pkgbase=mingw-w64-python-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-python-${_realname}")
pkgver=6.10a1
pkgrel=1
pkgdesc="Creates standalone executables from Python scripts, with the same performance (mingw-w64)"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
license=('PSF')
url="https://github.com/marcelotduarte/cx_Freeze/"
depends=("${MINGW_PACKAGE_PREFIX}-python"
         "${MINGW_PACKAGE_PREFIX}-python-cx-logging"
         "${MINGW_PACKAGE_PREFIX}-python-importlib-metadata"
         "${MINGW_PACKAGE_PREFIX}-python-lief")
makedepends=("${MINGW_PACKAGE_PREFIX}-gcc"
             "${MINGW_PACKAGE_PREFIX}-python-setuptools")
options=('staticlibs' 'strip' '!debug')
source=()
sha256sums=()

prepare() {
  rm -Rf python-${_realname}-${CARCH}
  git clone -b develop https://github.com/marcelotduarte/cx_Freeze.git python-${_realname}-${CARCH}
}
pkgver() {
  cd python-${_realname}-${CARCH}
  cat setup.cfg | grep '^version' | sed 's/[[:space:]]//g' | awk -F= '{print $2}'
}

build() {
  echo "Building for Python"
  cd python-${_realname}-${CARCH}
  ${MINGW_PREFIX}/bin/python setup.py build
}

package() {
  local _mingw_prefix=$(cygpath -wm ${MINGW_PREFIX})
  cd python-${_realname}-${CARCH}
  MSYS2_ARG_CONV_EXCL="--prefix=;--install-scripts=;--install-platlib=" \
    ${MINGW_PREFIX}/bin/python setup.py install --prefix=${MINGW_PREFIX} \
    --root="${pkgdir}" --optimize=1 --skip-build

  # fix python command in files
  for filename in "${pkgdir}${MINGW_PREFIX}/bin/cxfreeze*script.py"; do
    sed -e "s|${_mingw_prefix}/bin/||g" -i ${filename}
  done
}
