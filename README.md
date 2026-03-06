# QBZ APT Repository

APT repository for [QBZ](https://github.com/vicrodh/qbz), a native Qobuz client for Linux.

## Quick Install

```bash
# Add the GPG key
curl -fsSL https://vicrodh.github.io/qbz-apt/qbz-archive-keyring.gpg | sudo tee /usr/share/keyrings/qbz-archive-keyring.gpg > /dev/null

# Add the repository
echo "deb [signed-by=/usr/share/keyrings/qbz-archive-keyring.gpg arch=amd64] https://vicrodh.github.io/qbz-apt stable main" | sudo tee /etc/apt/sources.list.d/qbz.list

# Install
sudo apt update
sudo apt install qbz
```

### ARM64 (aarch64)

```bash
echo "deb [signed-by=/usr/share/keyrings/qbz-archive-keyring.gpg arch=arm64] https://vicrodh.github.io/qbz-apt stable main" | sudo tee /etc/apt/sources.list.d/qbz.list
```

## Update

```bash
sudo apt update && sudo apt upgrade qbz
```

## Supported Architectures

- `amd64` (x86_64)
- `arm64` (aarch64)

## How It Works

When a new release is published on [vicrodh/qbz](https://github.com/vicrodh/qbz), the release workflow triggers this repository to:

1. Download the `.deb` files from the GitHub release
2. Rebuild the APT repository index
3. Sign the repository with GPG
4. Deploy to GitHub Pages

## Setup (Maintainer)

### Required Secrets

| Secret | Description |
|---|---|
| `GPG_PRIVATE_KEY` | Armored GPG private key for signing the repository |
| `GH_PAT` | Personal access token with `repo` scope to read releases from `vicrodh/qbz` |

### Generate GPG Key

```bash
gpg --full-generate-key
# Choose: RSA and RSA, 4096 bits, no expiration
# Name: QBZ APT Signing Key
# Email: your-email

# Export private key (add to GH_PAT secret)
gpg --armor --export-secret-keys "QBZ APT Signing Key" > qbz-apt-signing-key.asc

# IMPORTANT: After adding to GitHub Secrets, delete the private key file
rm qbz-apt-signing-key.asc
```

### Enable GitHub Pages

1. Go to repository Settings > Pages
2. Source: Deploy from a branch
3. Branch: `gh-pages` / `/ (root)`
