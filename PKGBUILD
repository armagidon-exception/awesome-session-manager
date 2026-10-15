# Maintainer: armagidon <armagidon@armagidon.dev>
pkgname=awesome-session-manager-git
pkgver=r3.5453d88
pkgrel=1
pkgdesc='Systemd user session management for AwesomeWM'
arch=('any')
url='https://github.com/armagidon-exception/awesome-session-manager'
license=('AGPL-3.0-only')
depends=('bash' 'systemd' 'awesome')
makedepends=('git')
provides=('awesome-session-manager')
conflicts=('awesome-session-manager')
install="$pkgname.install"
source=("$pkgname::git+${url}.git")
sha256sums=('SKIP')

pkgver() {
    cd "$pkgname"
    printf 'r%s.%s' "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

package() {
    cd "$pkgname"

    install -Dm755 bin/awesome-session "$pkgdir/usr/bin/awesome-session"

    install -Dm644 -t "$pkgdir/usr/lib/systemd/user/" systemd/*.target systemd/*.service

    install -Dm644 xsessions/awesomewm-session.desktop \
        "$pkgdir/usr/share/xsessions/awesomewm-session.desktop"

    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
