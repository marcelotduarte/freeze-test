# Maintainer: Marcelo Duarte https://github.com/marcelotduarte

_name=cx_Freeze
_realname=cx-freeze
pkgbase=mingw-w64-python-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-python-${_realname}")
pkgver=6.14.0.dev1
pkgrel=1
pkgdesc="Creates standalone executables from Python scripts, with the same performance (mingw-w64)"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
license=('PSF')
url="https://github.com/marcelotduarte/cx_Freeze/"
depends=("${MINGW_PACKAGE_PREFIX}-python"
         "${MINGW_PACKAGE_PREFIX}-python-setuptools"
         "${MINGW_PACKAGE_PREFIX}-python-cx-logging"
         "${MINGW_PACKAGE_PREFIX}-python-lief")
makedepends=("${MINGW_PACKAGE_PREFIX}-cc"
             "${MINGW_PACKAGE_PREFIX}-tools"
             "${MINGW_PACKAGE_PREFIX}-python"
             "${MINGW_PACKAGE_PREFIX}-python-setuptools")
checkdepends=("${MINGW_PACKAGE_PREFIX}-python-nose"
              "${MINGW_PACKAGE_PREFIX}-python-pytest"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-cov"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-mock"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-timeout")
options=('staticlibs' 'strip' '!debug')
source=()
sha256sums=()

prepare() {
  #cd "${srcdir}/${_realname}-${pkgver}"
  rm -Rf "${_realname}-${pkgver}"
  git clone -b develop https://github.com/marcelotduarte/cx_Freeze.git "${_realname}-${pkgver}"
}

pkgver() {
  cd "${_realname}-${pkgver}"
  grep "__version__ = " cx_Freeze/__init__.py | sed 's/-/./' | awk -F\" '{print $2}'
}

build() {
  msg "Python build for ${MSYSTEM}"
  cp -r "${_realname}-${pkgver}" "python-build-${MSYSTEM}"
  cd "python-build-${MSYSTEM}"
  "${MINGW_PREFIX}"/bin/python setup.py build
}

package() {
  msg "Python install for ${MSYSTEM}"
  cd "python-build-${MSYSTEM}"
  echo "setup install --root=${pkgdir} --prefix=${MINGW_PREFIX}"
  MSYS2_ARG_CONV_EXCL="--prefix=;--install-scripts=;--install-platlib=" \
    "${MINGW_PREFIX}"/bin/python setup.py install --prefix="${MINGW_PREFIX}" \
      --root="${pkgdir}" --optimize=1 --skip-build

  # fix python command in files
  for _f in "${pkgdir}${MINGW_PREFIX}"/bin/*-script.py; do
    # Remove shebang line
    sed -e '1 { s/^#!.*$// }' -i "${_f}"
  done
}
