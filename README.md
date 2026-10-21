# ComfyUI snap

[![comfyui](https://snapcraft.io/comfyui/badge.svg)](https://snapcraft.io/comfyui)

Snap packaging for [ComfyUI](https://github.com/Comfy-Org/ComfyUI), a node-based interface for generative AI images, video and audio.

Unofficial build, not affiliated with or endorsed by the ComfyUI project.

## Build

```bash
snapcraft pack
```

## Install and try

<a href="https://snapcraft.io/comfyui"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install comfyui
comfyui
```

Open <http://127.0.0.1:8188>. Without a compatible NVIDIA GPU, run `comfyui --cpu` instead.

Models are not included. Put checkpoint files in `~/snap/comfyui/common/models/checkpoints`; inputs, outputs and settings also live under `~/snap/comfyui/common`.

```bash
comfyui --base-directory ~/ComfyUI
comfyui --listen 0.0.0.0 --port 8188
sudo snap connect comfyui:removable-media   # optional: /media, /mnt
```

## Known limitations

- GPU mode uses CUDA 13.0 and needs a compatible NVIDIA GPU and host driver. AMD ROCm, Intel GPU and Jetson-specific runtimes are not included; CPU mode is slow.
- Models and files must be in the snap's directories, a non-hidden home folder or connected removable storage.
- Custom nodes can use bundled dependencies. ComfyUI-Manager, extra Python packages, runtime compilation (such as `torch.compile`) and arbitrary host programs are not supported.
- The application and bundled frontend update through snapd. `--front-end-version` cannot install a different frontend into the read-only application directory.
- The browser interface has no built-in authentication; expose it only on a trusted network.

## Update to a new upstream release

Change `version` in `snap/snapcraft.yaml`, then rebuild.

## Automation

Builds, publishing, and dependency updates use [snap-automation](https://github.com/sidalit/snap-automation).

## License

The packaging is MIT-licensed (see `LICENSE`). ComfyUI is GPL-3.0-or-later-licensed; its dependencies carry their own licenses.
