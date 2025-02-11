# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Connor Behan <connor.behan@gmail.com>
# Contributor: Eric Bélanger <eric@archlinux.org>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _shell="sh"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _shell="bash"
fi
_proj="gtk"
_pkg=glib
pkgname="${_pkg}"
_majver="1.2"
_minver="10"
pkgver="${_majver}.${_minver}"
pkgrel=18
pkgdesc="Common C routines used by Gtk+ and other libs"
arch=(
  'i686'
  'x86_64'
  'armv6h'
  'arm'
  'aarch64'
  'armv7l'
  'mips'
  'pentium4'
)
url="http://www.${_proj}.org"
license=(
  'LGPL'
)
depends=(
  "${_libc}"
  "${_shell}"
)
options=(
  '!makeflags'
)
_http="https://download.gnome.org"
_ns="sources"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${_url}/${_majver}/${pkgname}-${pkgver}.tar.gz"
  "gcc340.patch"
  "aclocal-fixes.patch"
  "glib1-autotools.patch"
)
sha256sums=(
  '6e1ce7eedae713b11db82f11434d455d8a1379f783a79812cd2e05fc024a8d9f'
  'd62a2fdb0e8af99f5136fb7d86e0f6f24e5b17d08d8cf0a992772649dd9fff17'
  'acb153503489b747b7e41787d92a4ba5cd631e8f0dbf2fab14936ebeb49cbbc1'
  'ec4a932c20b2b79f69d6049c799d52740b59997b34c082c80d82640e27a2198f'
)

prepare() {
  cd \
    "${pkgname}-${pkgver}"
  patch \
    -Np1 \
    -i \
    "${srcdir}/gcc340.patch"
  patch \
    -Np0 \
    -i \
    "${srcdir}/aclocal-fixes.patch"
  patch \
    -Np1 \
    -i \
    "${srcdir}/glib1-autotools.patch"
  sed \
    -i \
    -e \
      's/ifdef[[:space:]]*__OPTIMIZE__/if 0/' \
    "glib.h"
  rm \
    "acinclude.m4"
}

build() {
  local \
    _config_flags=() \
    _configure_opts=()
  _configure_opts+=(
    --prefix="/usr"
    --mandir="/usr/share/man"
    --infodir="/usr/share/info"
  )
  if [[ $CARCH = "i686" ]]; then
    _config_flags+=(
      --host="i686-pc-linux-gnu"
      --target="i686-pc-linux-gnu"
    )
  elif [[ $CARCH = "x86_64" ]]; then
    _config_flags+=(
      --host="x86_64-unknown-linux-gnu"
      --target="x86_64-unknown-linux-gnu"
    )
  elif [[ $CARCH = "armv6h" ]]; then
    _config_flags+=(
    --host="armv6l-unknown-linux-gnueabihf"
    --target="armv6l-unknown-linux-gnueabihf"
    )
  fi
  cd \
    "${pkgname}-${pkgver}"
  autoreconf \
    --force \
    --install
  CFLAGS="-Wno-format-security" \
  ./configure \
    "${_configure_opts[@]}" \
    "${_config_flags[@]}"
  make
}

check() {
  cd \
    "${pkgname}-${pkgver}"
  make \
    check
}

package() {
  cd \
    "${pkgname}-${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    install
}

