# ComfyUI GUI Installer for Void Linux

## What it does

1. **Checks dependencies** — verifies `zenity`, `git`, and `python` are installed, and offers to install any missing ones via `xbps` (requires `doas`).
2. **Prompts for an install directory**.
3. **Prompts for your GPU type** — NVIDIA, AMD, or CPU-only — and installs the matching PyTorch build.
4. **Clones ComfyUI**, creates a Python virtual environment, installs PyTorch and ComfyUI's dependencies.
5. **Generates a Fish launcher** (`run_comfyui.fish`) in the install directory so you can start ComfyUI with a single command afterward.
6. **Reports success or failure** in a dialog, with a log file at `/tmp/comfyui_install.log` for troubleshooting.

## Requirements

- Void Linux (or a Void-based distro with `xbps`)
- [Fish shell](https://fishshell.com/)
- `doas` access, if `zenity`, `git`, or `python` aren't already installed

## Usage

```bash
git clone https://github.com/pixelvulp/void-comfyui-installer.git
cd /path/ComfyUI/
chmod +x install_comfyui.sh
./install_comfyui.sh
```

Follow the dialogs: pick an install directory, pick your GPU type, and let it run. Once finished, start ComfyUI with:

```fish
/path/ComfyUI/run_comfyui.fish
```

## Known issues & troubleshooting

### `Fatal Python error: Illegal instruction` on startup

This comes from `kornia_rs`, a dependency pulled in by ComfyUI's built-in post-processing nodes. The published `kornia_rs` wheel is compiled assuming at least AVX/AVX2/FMA3 CPU support. On older CPUs (pre-2011, before AVX existed) importing it crashes the whole process before ComfyUI even starts. this, however, was fixed in v1.1.0. if your build predates v1.1.0 run

```fish
source venv/bin/activate.fish
pip uninstall -y kornia kornia_rs
```

### GPU not fully utilized / "does not include kernels for this GPU" warning

The default installer runs `pip install torch torchvision torchaudio`, which currently pulls a CUDA 13.0 build. That build drops kernel support for older NVIDIA compute capabilities (e.g. Pascal-generation cards like the GTX 10-series, compute capability 6.x). If you see a warning like:

```
Your installed torch==X.Y.Z+cu130 does not include kernels for this GPU.
```

Reinstall PyTorch against an older CUDA build that still supports your card

(Check [pytorch.org/get-started/locally](https://pytorch.org/get-started/locally/) for the CUDA build matching your specific GPU generation.)

## Log file

All install output is captured in `/tmp/comfyui_install.log`. If the installer reports a failure, check this file first for the underlying error.

## License

MIT.
