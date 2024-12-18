# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Danilo J. S. Bellini <danilo dot bellini at gmail dot com>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Douglas Soares de Andrade <dsa@aur.archlinux.org>
# Contributor: Angel 'angvp' Velasquez <angvp[at]archlinux.com.ve>

_py="python2"
_pkg="numpy"
pkgname="${_py}-${_pkg}"
pkgver=1.16.6
pkgrel=3
pkgdesc="Scientific tools for Python 2"
arch=(
  'x86_64'
  'i686'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
)
license=(
  'custom'
)
url="https://www.${_pkg}.org"
depends=(
  'cblas'
  'lapack'
  "${_py}"
)
optdepends=(
  'blas-openblas: faster linear algebra'
)
makedepends=(
  "${_py}-setuptools"
  'gcc-fortran'
  'cython2'
)
_http="https://github.com"
_ns="${_pkg}"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${pkgname}-${pkgver}.tar.gz::${_url}/archive/v${pkgver}.tar.gz"
)
sha512sums=(
  '27344ae10c7e7fd222b34d1514aaccbb62d1322cc79bec28f5c1e8741eefbb6bc7664055e49ed4f08d5716ca93d4c997fb98bee26a19957543ab4f8c15cd41f7'
)

prepare() {
  cd \
    "${_pkg}-${pkgver}"
  sed \
    -e "s|#![ ]*/usr/bin/python$|#!/usr/bin/python2|" \
    -e "s|#![ ]*/usr/bin/env python$|#!/usr/bin/env python2|" \
    -e "s|#![ ]*/bin/env python$|#!/usr/bin/env python2|" \
    -i \
    $(find \
      "." \
      -name \
        '*.py'\
      -type \
        "f")
}

_bin_get() {
  local \
    _bin \
    _gcc
  _gcc="$( \
    command \
      -v \
      "gfortran")"
  _bin="$( \
    dirname \
      "${_gcc}")"
  echo \
    "${_bin}"
}

_usr_get() {
  local \
    _bin
  _bin="$( \
    _bin_get)"
  echo \
    "$(dirname \
      "${_bin}")"
}

_include_get() {
  local \
    _usr
  _usr="$( \
    _usr_get)"  
  echo \
    "${_usr}/include"
}

build() {
  local \
    _cflags=() \
    _include
  _include="$( \
    _include_get)"
  _cflags+=(
    "${CFLAGS}"
    -ffat-lto-objects
  )
  cd \
    "${_pkg}-${pkgver}"
  CFLAGS="${_cflags[*]}" \
  "${_py}" \
    setup.py \
      build
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      install \
        --prefix='/usr' \
	--root="${pkgdir}" \
	--optimize=1
  install \
    -Dm644 \
    "LICENSE.txt" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  mv \
    "$pkgdir/usr/bin/f2py"{,2}
}
