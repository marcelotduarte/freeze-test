# Maintainer: Marcelo Duarte https://github.com/marcelotduarte

_name=cx_Freeze
_realname=cx-freeze
pkgbase=mingw-w64-python-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-python-${_realname}")
pkgver=6.16.0
pkgrel=1
pkgdesc="Creates standalone executables from Python scripts, with the same performance (mingw-w64)"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
license=('PSF')
url="https://github.com/marcelotduarte/cx_Freeze/"
depends=("${MINGW_PACKAGE_PREFIX}-python"
         "${MINGW_PACKAGE_PREFIX}-python-setuptools"
         "${MINGW_PACKAGE_PREFIX}-python-wheel"
         "${MINGW_PACKAGE_PREFIX}-python-cx-logging"
         "${MINGW_PACKAGE_PREFIX}-python-lief")
makedepends=("${MINGW_PACKAGE_PREFIX}-python-build"
             "${MINGW_PACKAGE_PREFIX}-python-installer"
             "${MINGW_PACKAGE_PREFIX}-python-wheel"
             "${MINGW_PACKAGE_PREFIX}-cc"
             "${MINGW_PACKAGE_PREFIX}-tools")
checkdepends=("${MINGW_PACKAGE_PREFIX}-python-nose"
              "${MINGW_PACKAGE_PREFIX}-python-pytest"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-cov"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-mock"
              "${MINGW_PACKAGE_PREFIX}-python-pytest-timeout")
options=('staticlibs' 'strip' '!debug')
source=()
sha256sums=()

prepare() {
  rm -Rf "${srcdir}"/python-${_realname}-${MSYSTEM}
  git clone -b develop https://github.com/marcelotduarte/cx_Freeze.git "${srcdir}"/python-${_realname}-${MSYSTEM}
  cd "${srcdir}"/python-${_realname}-${MSYSTEM}
  # ignore version check for setuptools
  sed -i 's/"setuptools>=.*"/"setuptools"/' pyproject.toml
}

pkgver() {
  cd python-${_realname}-${MSYSTEM}
  grep "__version__ = " cx_Freeze/__init__.py | sed 's/-/./' | awk -F\" '{print $2}'
}

build() {
  cd python-${_realname}-${MSYSTEM}
  ${MINGW_PREFIX}/bin/python -m build --wheel --skip-dependency-check --no-isolation
}

package() {
  cd python-${_realname}-${MSYSTEM}
  MSYS2_ARG_CONV_EXCL="--prefix=" \
    ${MINGW_PREFIX}/bin/python -m installer --prefix=${MINGW_PREFIX} \
    --destdir="${pkgdir}" dist/*.whl
}
