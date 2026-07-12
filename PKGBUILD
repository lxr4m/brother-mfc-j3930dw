# Maintainer: LxR4m
#
# Proprietary Brother printer driver for MFC-J3930DW.
# Downloads original driver packages directly from Brother.
#
_model=mfcj3930dw
_device_name=MFC-J3930DW
pkgname=brother-mfc-j3930dw
pkgver=1.0.1
pkgrel=1
pkgdesc="LPR driver and CUPS wrapper for Brother ${_device_name}"
arch=("i686" "x86_64")
url="https://support.brother.com/g/b/producttop.aspx?c=as_ot&lang=en&prod=mfcj3930dw_eu_as_cn"
license=("LicenseRef-Brother-EULA")
source=(
    "mfcj3930dwlpr-${pkgver}-0.i386.deb::https://download.brother.com/welcome/dlf103014/mfcj3930dwlpr-${pkgver}-0.i386.deb"
    "mfcj3930dwcupswrapper-${pkgver}-0.i386.deb::https://download.brother.com/welcome/dlf103038/mfcj3930dwcupswrapper-${pkgver}-0.i386.deb"
	"LICENSE_ENG.txt"
)

noextract=(
    "mfcj3930dwlpr-${pkgver}-0.i386.deb"
    "mfcj3930dwcupswrapper-${pkgver}-0.i386.deb"
)

sha256sums=(
    '9775622b7749397361c3a56c6ad85763ff3dc783561de649d77a912b444c0047'
    'a0098ce2d9d61b343bf72cb1931215dd22eb1373cbf9501027b76d53e9714cf2'
    '46b2902ead35bd6d1a111740022133cb120bc7c32a2effdc8d381cc25a59dfa6'
)
depends=('bash' 'perl' 'cups')
depends_x86_64=('lib32-glibc' 'lib32-gcc-libs')
optdepends=(
    'brscan5: scanner support'
    'system-config-printer: graphical printer configuration tool'
)

prepare() {
    mkdir -p "${srcdir}"/{lpr,cups}

    (
        cd "${srcdir}/lpr"
        ar x "${srcdir}/mfcj3930dwlpr-${pkgver}-0.i386.deb"
        tar -xf data.tar.gz
    )

    (
        cd "${srcdir}/cups"
        ar x "${srcdir}/mfcj3930dwcupswrapper-${pkgver}-0.i386.deb"
        tar -xf data.tar.gz
    )
}


package() {
    local SRC_PREFIX="/opt" # unfortunately, /opt is hard-coded into driver binaries and cannot be replaced

    # Copy contents from both Debian packages
    install -dm755 "${pkgdir}${SRC_PREFIX}"

    cp -a "${srcdir}/lpr/opt/brother"  "${pkgdir}${SRC_PREFIX}/"
    cp -a "${srcdir}/cups/opt/brother" "${pkgdir}${SRC_PREFIX}/"

    install -dm755 "${pkgdir}/usr/bin"

    cp -a \
        "${srcdir}/lpr/usr/bin/brprintconf_${_model}" \
        "${pkgdir}/usr/bin/"

    # Removing deprecated setupPrintcap
    rm -f "${pkgdir}${SRC_PREFIX}/brother/Printers/${_model}/inf/setupPrintcapij"

    # Fix permissions
    find "${pkgdir}${SRC_PREFIX}/brother" -type d -exec chmod 755 {} +

    find "${pkgdir}${SRC_PREFIX}/brother" -type f -exec chmod 644 {} +

    chmod 755 \
         "${pkgdir}${SRC_PREFIX}/brother/Printers/${_model}/lpd/brmfcj3930dwfilter"

    chmod 755 \
	    "${pkgdir}${SRC_PREFIX}/brother/Printers/${_model}/lpd/filter_mfcj3930dw"

    chmod 755 \
        "${pkgdir}${SRC_PREFIX}/brother/Printers/${_model}/cupswrapper/cupswrapper${_model}"

    chmod 755 \
  		"${pkgdir}${SRC_PREFIX}/brother/Printers/${_model}/cupswrapper/brother_lpdwrapper_${_model}"

    chmod 755 \
        "${pkgdir}/usr/bin/brprintconf_${_model}"

    # CUPS filter
    install -dm755 "${pkgdir}/usr/lib/cups/filter"

    ln -s \
        "${SRC_PREFIX}/brother/Printers/${_model}/cupswrapper/brother_lpdwrapper_${_model}" \
        "${pkgdir}/usr/lib/cups/filter/brother_lpdwrapper_${_model}"  

    install -Dm644 \
        "${srcdir}/LICENSE_ENG.txt" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

