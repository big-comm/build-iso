# BigCommunity ISO Builder

<div align="center">
  <h3>Automated Build System for BigCommunity and BigLinux ISO Images</h3>
</div>

## 📖 Overview

The BigCommunity ISO Builder is a comprehensive workflow and action system designed to automate the build process of custom Linux installation images. It supports creating ISOs for both BigCommunity and BigLinux distributions with various desktop environments, kernel options, and configuration profiles.

This tool streamlines the typically complex process of building distribution ISOs, providing an easy-to-use GitHub Actions workflow that handles the entire build pipeline from configuration to release.

## ✨ Features

- **Multiple Distribution Support**: Build ISOs for BigCommunity or BigLinux
- **Desktop Environment Options**: Choose from KDE, XFCE, Gnome, Cinnamon, Hyprland, and more
- **Kernel Flexibility**: Select from latest, LTS, old LTS, or Xanmod kernels
- **Branch Management**: Build from stable, testing, or development branches
- **Automated Releases**: Creates GitHub releases with proper versioning
- **Telegram Notifications**: Provides build status updates via Telegram
- **Debugging Support**: Optional tmate integration for debugging builds
- **MD5 Hash Generation**: Automatically generates verification hashes
- **Progress Tracking**: Step-by-step reporting of the build process
- **Split Archive Creation**: Creates split archives for larger ISOs

## 🚀 Usage

### GitHub Actions Workflow

To use this ISO builder in your GitHub repository:

1. Add the workflow file (`edition.yml`) to your `.github/workflows/` directory
2. Configure the GitHub repository secrets:
   - `TOKEN_BOT`: Telegram bot token for notifications
   - `CHAT_ID`: Telegram chat ID for notifications
   - `TOKEN_RELEASE`: GitHub token with permissions to create releases
   - `GITHUB_TOKEN`: Standard GitHub token

3. Trigger the workflow manually with your desired parameters:
   - Distribution name
   - Edition (desktop environment)
   - Kernel version
   - Branch selections
   - Other configuration options

### Manual Trigger Example

You can trigger a build from the GitHub Actions UI by selecting:

- **Distribution**: bigcommunity
- **Edition**: xfce
- **Kernel**: lts
- **Branch**: testing
- **Scope**: full

## 🔧 Configuration Options

### Distribution Options
- `bigcommunity`: The BigCommunity Linux distribution
- `biglinux`: The BigLinux distribution

### Desktop Environments
- `kde`: KDE Plasma desktop
- `xfce`: XFCE desktop environment
- `gnome`: GNOME desktop environment
- `cinnamon`: Cinnamon desktop
- `deepin`: Deepin desktop environment
- `cosmic`: COSMIC desktop environment
- `hyprland`: Hyprland compositor
- `wmaker`: Window Maker window manager

### Kernel Options
- `latest`: Latest available kernel
- `lts`: Long-term support kernel
- `oldLts`: Previous LTS kernel version
- `xanmod`: Xanmod performance-optimized kernel

### Branch Selection
- `stable`: Stable release branch
- `testing`: Testing/beta branch
- `unstable`: Development branch (Manjaro only)

## 📦 Output

The build process produces:
- ISO file with standardized naming: `[distro]_[branch]_[edition]_[date]_k[kernel].iso`
- MD5 hash file for verification
- Package list file (.pkgs)
- Split 7z archives for easier downloading

## 🔄 Workflow Process

1. **Setup Phase**: Prepares the environment and sends initial notification
2. **Build Phase**: Builds the ISO using the specified parameters
3. **Release Phase**: Creates GitHub release with artifacts and sends completion notification

## 🛠️ Implementation Details

The ISO builder consists of three main components:

1. **GitHub Action** (`action.yml`): Defines the custom action that performs the actual ISO build
2. **Workflow File** (`edition.yml`): Coordinates the entire build process from start to finish
3. **Build Script** (`build-iso.sh`): Contains the core functionality for building the ISO

## 🔍 Debugging

If you encounter issues during the build process:

1. Enable the tmate option when triggering the workflow
2. Connect to the provided SSH session to inspect the build environment
3. Check the build logs and Telegram notifications for error messages

## 🤝 Contributing

Contributions to improve the ISO builder are welcome! Please feel free to submit issues or pull requests to the repository.

## 📄 License

This project is open source and available under the terms of the GPL v3 license.

## 🔗 Links

- [BigCommunity Website](https://communitybig.org)
- [BigLinux Website](https://www.biglinux.com.br)
- [ISO Profiles Repository](https://github.com/big-comm/iso-profiles)
