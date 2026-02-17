# os-blocky-tfenby

Custom OPNsense package repository for the Blocky DNS ad-blocker plugin.

## What's in this repo

This is a FreeBSD `pkg` repository served via GitHub Pages.

Two packages are provided:

- **`blocky-tfenby`** — Blocky DNS proxy binary (upstream: [0xERR0R/blocky](https://github.com/0xERR0R/blocky)), repackaged for FreeBSD
- **`os-blocky-tfenby`** — OPNsense plugin providing UI integration, service control, and configuration management

Currently ships Blocky v0.28.2 for FreeBSD 14 amd64.

## Installation

Run the following commands on your OPNsense box.

**1. Install the repository signing key:**

```sh
mkdir -p /usr/local/etc/pkg/keys
fetch -o /usr/local/etc/pkg/keys/tfenby.pub https://tfenby.github.io/opnsense-blocky/repo.pub
```

**2. Install the repository config:**

```sh
fetch -o /usr/local/etc/pkg/repos/os-blocky-tfenby.conf https://tfenby.github.io/opnsense-blocky/repo-tfenby.conf
```

**3. Install the plugin:**

```sh
pkg update
pkg install os-blocky-tfenby
```

After installation, enable Blocky under **Services** in the OPNsense web UI.

## Uninstall

```sh
pkg remove os-blocky-tfenby blocky-tfenby
rm /usr/local/etc/pkg/repos/os-blocky-tfenby.conf
rm /usr/local/etc/pkg/keys/tfenby.pub
```

## Package Verification

Two trust layers are available.

**Repository signing**

The package catalog is signed with an RSA key. `pkg update` verifies the signature automatically if the signing key is installed (step 1 above). The signing key (`repo.pub`) is also GPG-signed by the maintainer — verify with:

```sh
fetch https://tfenby.github.io/opnsense-blocky/repo.pub.asc
gpg --verify repo.pub.asc repo.pub
```

**Build provenance**

Packages are built by GitHub Actions with SLSA build provenance attestations. Verify any downloaded `.pkg` file:

```sh
gh attestation verify <package>.pkg --repo TFenby/opnsense-blocky-source
```

**Upstream binary integrity**

The Blocky binary is checksum-verified against upstream release checksums during CI before packaging.

## Source Code

- Build scripts, CI/CD, and plugin source: [TFenby/opnsense-blocky-source](https://github.com/TFenby/opnsense-blocky-source)
- Upstream Blocky: [0xERR0R/blocky](https://github.com/0xERR0R/blocky)

## License

- Plugin: BSD-2-Clause
- Blocky: Apache-2.0
