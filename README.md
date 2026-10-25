<div align="center">

<img src="snap/gui/comfyui.png" alt="ComfyUI logo" width="128" />

# ComfyUI

**Create images, video and audio with node-based AI workflows.**

[![comfyui](https://snapcraft.io/comfyui/badge.svg)](https://snapcraft.io/comfyui)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [ComfyUI](https://github.com/Comfy-Org/ComfyUI), a generative AI server with a browser interface and optional NVIDIA GPU acceleration.

</div>

---

## Getting Started

<a href="https://snapcraft.io/comfyui"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install comfyui
```

The server starts automatically at <http://127.0.0.1:8188>. You can also connect using [Comfy Desktop](https://github.com/sidalit/comfy-desktop-snap).

Models are downloaded separately: open **Manager → Model Manager**, select **Install**, then refresh the model list. Downloads are stored in `/var/snap/comfyui/common/models`.

## Configure

| Option | Accepted values | Default |
| --- | --- | --- |
| `daemon` | `true`, `false` | `true`: start now and on boot. |
| `listen` | IPv4 or IPv6 address, without a zone identifier. | `127.0.0.1` |
| `port` | Integer from `1` to `65535`. | `8188` |
| `accelerator` | `auto`, `cpu`, `cuda` | `auto`: request CUDA when an NVIDIA GPU is detected; otherwise use CPU. |

```bash
sudo snap set comfyui accelerator=cpu
sudo snap set comfyui listen=127.0.0.1 port=8188
sudo snap get comfyui
sudo snap logs comfyui.daemon
```

The accelerator setting adds or removes the `torch-cuda` component. Initial GPU startup waits for its download; the server uses CPU if CUDA is unavailable. To start the service manually instead of automatically:

```bash
sudo snap set comfyui daemon=false
sudo snap start comfyui.daemon
```

Service data lives in `/var/snap/comfyui/common`. The foreground command uses separate data in `~/snap/comfyui/common`; run `comfyui --cpu --port 8189` alongside the service. Optional removable-drive access for this command: `sudo snap connect comfyui:removable-media`.

## Limitations

- CUDA requires a compatible NVIDIA GPU, driver and snapd 2.68 or later. AMD ROCm, Intel GPU and Jetson-specific runtimes are not included.
- Model Manager downloads work with the default loopback listener; other addresses follow its security restrictions.
- Custom nodes can use bundled dependencies. Extra Python packages, runtime compilation and arbitrary host programs are unsupported.
- The service cannot access your home or removable drives; copy models into its model directory. Ordinary browser downloads do not install models there.
- Desktop and CLI clients cannot start a stopped server. The application and bundled frontend update through snapd.
- The browser interface has no built-in authentication; use trusted networks only.

## Build

```bash
git clone https://github.com/sidalit/comfyui-snap.git
cd comfyui-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/comfyui-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/Comfy-Org/ComfyUI/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/comfyui-snap/blob/main/LICENSE).
