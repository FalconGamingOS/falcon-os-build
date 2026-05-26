# falcon-os-build

ISO build pipeline for [FalconOS](https://falcongamingos.github.io) — Ubuntu 24.04-based console-style gaming OS.

## What this repo does

Produces a bootable FalconOS `.iso` from a stock Ubuntu 24.04 base using Cubic. Everything that ends up in the final ISO lives here: kernel config, package selection, gaming stack setup, GPU driver automation, Gamescope session, and Calamares installer branding.

## Stack

| Tool | Purpose |
|---|---|
| Cubic | Ubuntu ISO customisation (chroot-based) |
| Bash | All chroot scripts |
| Liquorix | Low-latency gaming kernel |
| Calamares | Graphical installer |
| QEMU/KVM | ISO testing |

## Directory structure

```
falcon-os-build/
├── cubic-scripts/
│   ├── 01-base-hardening.sh     # Remove bloat, kill snap, disable services
│   ├── 02-nvidia-setup.sh       # GPU detection + driver automation service
│   ├── 03-gaming-stack.sh       # Steam, Proton-GE, Lutris, DXVK, GameMode
│   ├── 04-gamescope-session.sh  # Wayland session + LightDM auto-login
│   └── 05-calamares.sh          # Installer branding + partition config
├── config/
│   ├── sysctl/99-falcon.conf    # Gaming kernel parameters
│   └── udev/60-falcon-io.rules  # NVMe I/O scheduler
├── calamares/
│   ├── branding/falconos/       # Logo, name, slideshow
│   └── modules/                 # Partition, packages, post-install
├── tests/
│   └── run-tests.sh             # Automated post-install verification
├── docs/
│   ├── dev-notes.md             # ISO structure notes
│   └── benchmarks.md            # Boot time + RAM measurements
└── build.sh                     # One-command ISO build
```

## Building the ISO

```bash
# Prerequisites
sudo apt install cubic qemu-kvm virt-manager

# Clone and build
git clone https://github.com/FalconGamingOS/falcon-os-build
cd falcon-os-build
./build.sh --iso ~/iso/ubuntu-24.04-desktop-amd64.iso --output ~/output/
```

## Testing

```bash
# Boot ISO in QEMU (one command)
./tests/qemu-test.sh ~/output/falconos-0.1.iso

# Run post-install test suite (on real hardware after install)
./tests/run-tests.sh
```

## Minimum target hardware

```
CPU      Intel Core i5 10th gen+
RAM      16 GB DDR4/DDR5
Storage  NVMe M.2
GPU      NVIDIA RTX 30xx (Ampere)
```

## Related repos

- [falcon-launcher](https://github.com/FalconGamingOS/falcon-launcher) — React UI baked into this ISO
- [falcon-system-service](https://github.com/FalconGamingOS/falcon-system-service) — Python FastAPI backend baked into this ISO
- [falcon-hw-profiles](https://github.com/FalconGamingOS/falcon-hw-profiles) — Hardware profiles applied by 02-nvidia-setup.sh

## License

GPL v3.0
