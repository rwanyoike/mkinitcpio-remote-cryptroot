pkgname=mkinitcpio-remote-cryptroot
pkgver=0.0.1
pkgrel=1
pkgdesc="..."
arch=("any")
url="https://github.com/rwanyoike/mkinitcpio-remote-cryptroot"
license=("MIT")
depends=("openssh" "tinyssh" "tinyssh-keyconvert")
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/master.tar.gz")
sha512sums=("SKIP")

package() {
    install -Dm644 "${srcdir}/${pkgname}-master/initrd-network.service" "${pkgdir}/usr/lib/systemd/system/initrd-network.service"
    install -Dm644 "${srcdir}/${pkgname}-master/initrd-network_install" "${pkgdir}/usr/lib/initcpio/install/network"
    install -Dm644 "${srcdir}/${pkgname}-master/initrd-tinyssh.service" "${pkgdir}/usr/lib/systemd/system/initrd-tinyssh.service"
    install -Dm644 "${srcdir}/${pkgname}-master/initrd-tinyssh_install" "${pkgdir}/usr/lib/initcpio/install/tinyssh"
    install -Dm644 "${srcdir}/${pkgname}-master/initrd-password-agent_install" "${pkgdir}/usr/lib/initcpio/install/password-agent"
    install -Dm644 "${srcdir}/${pkgname}-master/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
