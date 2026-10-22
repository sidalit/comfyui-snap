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
sudo snap services comfyui
```

The server starts automatically at <http://127.0.0.1:8188>. It uses a compatible NVIDIA GPU when available and otherwise falls back to CPU mode.

```bash
sudo snap set comfyui daemon=true   # default: start now and on boot
sudo snap set comfyui daemon=false  # stop and disable automatic startup
sudo snap start comfyui.daemon      # start manually while automatic startup is disabled
sudo snap set comfyui listen=127.0.0.1 port=8188
sudo snap set comfyui cpu=true    # force CPU mode
sudo snap set comfyui cpu=false   # automatic GPU/CPU selection
sudo snap get comfyui
sudo snap logs comfyui.daemon
```

Closing the desktop leaves the server running; opening it cannot start a stopped server.

Models are not included. Download them from [Comfy Desktop](https://github.com/sidalit/comfyui-desktop-snap) or the browser:

1. Open **Manager → Model Manager**.
2. Search for a model and click **Install**.
3. Click **Refresh**, then select the model in your workflow.

The server downloads models into `/var/snap/comfyui/common/models` automatically. No folder selection or permission setup is needed. Use Model Manager; ordinary workflow download links save files to your computer instead.

You can also copy models manually, including over SSH:

```bash
sudo mkdir -p /var/snap/comfyui/common/models/checkpoints
sudo cp model.safetensors /var/snap/comfyui/common/models/checkpoints/
```

For the desktop application, install [comfyui-desktop](https://github.com/sidalit/comfyui-desktop-snap); it uses the server's configured address and port when launched. Reopen it after changing these settings.

The foreground command keeps its own data in `~/snap/comfyui/common`. Use another port while the service is running:

```bash
comfyui --cpu --port 8189
comfyui --base-directory ~/ComfyUI --port 8189
sudo snap connect comfyui:removable-media   # optional: /media, /mnt
```

Existing per-user data is preserved; copy any models you want the service to use into its model directory.

## Known limitations

- GPU mode uses CUDA 13.0 and needs a compatible NVIDIA GPU and host driver. AMD ROCm, Intel GPU and Jetson-specific runtimes are not included; CPU mode is slow.
- The service uses its own data directory. Home folders and connected removable storage are available to the foreground command.
- Model Manager installs models from its catalogue. Downloads work with the default localhost listener; other listening addresses follow Manager's security restrictions.
- Custom nodes can use bundled dependencies. Installing extra Python packages, runtime compilation (such as `torch.compile`) and arbitrary host programs are not supported.
- The application and bundled frontend update through snapd. `--front-end-version` cannot install a different frontend into the read-only application directory.
- The browser interface has no built-in authentication; expose it only on a trusted network.

## Update to a new upstream release

Change `version` in `snap/snapcraft.yaml`, then rebuild.

## Automation

Builds, publishing, and dependency updates use [snap-automation](https://github.com/sidalit/snap-automation).

## License

The packaging is MIT-licensed (see `LICENSE`). ComfyUI is GPL-3.0-or-later-licensed; its dependencies carry their own licenses.
