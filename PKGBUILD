# Maintainer: pixelvulp <https://github.com/pixelvulp>
pkgname=void-comfyui-installer
pkgver=1.0.0
pkgrel=1
pkgdesc="GUI installer script for ComfyUI on Void Linux (Fish Shell Edition)"
void=('any')
url="https://github.com/pixelvulp/void-comfyui-installer"
license=('MIT')
depends=('bash' 'zenity' 'git' 'python' 'fish')
optdepends=(
  'nvidia-utils: NVIDIA GPU support'
  'cuda: CUDA backend for PyTorch'
  'rocm-core: AMD ROCm GPU support'
)
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('SKIP')

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  install -Dm755 install_comfyui.sh "${pkgdir}/usr/bin/void-comfyui-installer"

  if [ -f README.md ]; then
    install -Dm644 README.md "${pkgdir}/usr/share/doc/${pkgname}/README.md"
  fi
}
