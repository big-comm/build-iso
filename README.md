# BigCommunity ISO Builder

<div align="center">
  <img src="https://img.shields.io/badge/Version-3.0.1-blue.svg" alt="Version"/>
  <img src="https://img.shields.io/badge/Platform-GitHub_Actions-green.svg" alt="Platform"/>
  <img src="https://img.shields.io/badge/License-GPL_v3-red.svg" alt="License"/>
</div>

<div align="center">
  <h3>🚀 Automated Build System for BigCommunity and BigLinux ISO Images</h3>
  <p>A powerful, plug-and-play GitHub Actions workflow for building custom Linux distribution ISOs with dynamic content discovery and multi-organization support.</p>
</div>

## 📖 Overview

The BigCommunity ISO Builder is a sophisticated automation system that streamlines the creation of custom Linux installation images. Built on GitHub Actions, it features a **plug-and-play architecture** that automatically discovers available build options from ISO profile repositories via GitHub API, making it incredibly easy for any organization to create their own custom Linux distributions.

This system eliminates the complexity of manual ISO building by providing an intelligent workflow that handles everything from environment setup to final release distribution, with comprehensive monitoring and notification capabilities.

## ✨ Key Features

### 🔧 **Plug-and-Play Architecture**
- **Zero-configuration setup** for new organizations
- **Dynamic content discovery** via GitHub API
- **Automatic detection** of available build directories and desktop editions
- **Real-time validation** against live repository contents

### 🎯 **Multi-Distribution Support**
- **BigCommunity Linux**: Community-driven Manjaro-based distribution
- **BigLinux**: Performance-optimized Brazilian Linux distribution
- **Extensible framework** for additional distributions

### 🖥️ **Desktop Environment Flexibility**
- Automatically discovers available editions from ISO profiles
- Common options: KDE, XFCE, GNOME, Cinnamon, Hyprland, Deepin, COSMIC
- **No hardcoded limitations** - supports any edition in your profiles

### ⚡ **Advanced Build Options**
- **Kernel Selection**: Latest, LTS, Old LTS, Xanmod variants
- **Branch Management**: Stable, Testing, Development builds
- **Compression Optimization**: Automatic level adjustment based on branch
- **Custom Repository Integration**: Community and BigLinux package sources

### 📱 **Comprehensive Monitoring**
- **Real-time Telegram notifications** with build progress
- **Detailed logging** with colored terminal output
- **GitHub Actions integration** with artifact management
- **Automatic MD5 verification** and package manifests

### 🐛 **Debugging & Development**
- **tmate integration** for live debugging sessions
- **Verbose build output** with step-by-step tracking
- **Environment inspection** tools and diagnostics

## 🛠️ Prerequisites

### Required Software
- **GitRepo Package**: Essential for triggering ISO builds
  ```bash
  # Install the GitRepo package from:
  # https://github.com/big-comm/gitrepo
  
  # The package includes:
  # - build-iso: Command-line tool for ISO generation
  # - Interactive menu system
  # - Organization management
  # - Automatic GitHub Actions triggering
  ```

### GitHub Repository Setup
1. **Secrets Configuration**:
   - `TOKEN_BOT`: Telegram bot token for build notifications
   - `CHAT_ID`: Telegram chat ID for status updates  
   - `TOKEN_RELEASE`: GitHub personal access token with repository and workflow permissions
   - `GITHUB_TOKEN`: Automatically provided by GitHub Actions

2. **Repository Structure**:
   ```
   your-repo/
   ├── .github/workflows/edition.yml    # Main workflow file
   ├── action.yml                       # Custom action definition
   ├── build-iso.sh                     # Core build script
   └── README.md                        # Documentation
   ```

## 🚀 Quick Start

### Method 1: Using GitRepo (Recommended)

1. **Install GitRepo package**:
   ```bash
   # Download and install from:
   # https://github.com/big-comm/gitrepo
   sudo pacman -U gitrepo-*.pkg.tar.zst
   ```

2. **Run Interactive Mode**:
   ```bash
   build-iso
   ```

3. **Automatic Mode**:
   ```bash
   build-iso -o your-organization --auto
   ```

### Method 2: Manual GitHub Actions Trigger

The workflow is designed to be triggered programmatically via the GitRepo tool, but can also be triggered via repository dispatch events with proper payload formatting.

## ⚙️ Configuration

### Adding Your Organization

To add your organization to the ISO builder, simply edit the `config.py` file in the GitRepo package:

```python
ORG_DEFAULT_CONFIGS = {
    # Existing organizations...
    
    "your-organization": {
        "distroname": "bigcommunity",  # or "biglinux"
        "iso_profiles_repo": "https://github.com/your-org/iso-profiles",
        "branches": {
            "manjaro": "stable",      # stable, testing, unstable
            "community": "stable",    # stable, testing, unstable (leave "" if not used)
            "biglinux": "stable"      # stable, testing, unstable (leave "" if not used)
        },
        "kernel": "latest",           # latest, lts, oldlts, xanmod
        "build_dir": "bigcommunity",  # directory name in your iso-profiles
        "edition": "xfce"             # your preferred default desktop
    }
}
```

### Configuration Parameters

| Parameter | Description | Options |
|-----------|-------------|---------|
| `distroname` | Base distribution | `bigcommunity`, `biglinux` |
| `iso_profiles_repo` | ISO profiles repository URL | Any valid GitHub repository |
| `manjaro_branch` | Manjaro base system branch | `stable`, `testing`, `unstable` |
| `community_branch` | BigCommunity customizations | `stable`, `testing`, `""` |
| `biglinux_branch` | BigLinux customizations | `stable`, `testing`, `""` |
| `kernel` | Default kernel type | `latest`, `lts`, `oldlts`, `xanmod` |
| `build_dir` | ISO profiles directory | Validated against repository |
| `edition` | Default desktop environment | Validated against repository |

## 📦 Build Output

The ISO builder generates several artifacts:

### Primary Outputs
- **ISO Image**: `[distro]_[type]_[edition]_[date]_k[kernel].iso`
- **MD5 Hash**: Verification checksum for integrity
- **Package Manifest**: Complete list of included packages
- **Split Archives**: 7z compressed files for easier distribution

### Build Types
- **STABLE**: `stable/stable` branch combination
- **BETA**: `stable/testing` or mixed stable/testing
- **DEVELOPMENT**: Any unstable branch combination

### Example Output
```
bigcommunity_STABLE_xfce_2025-01-15_klatest.iso
bigcommunity_STABLE_xfce_2025-01-15_klatest.iso.md5
bigcommunity_STABLE_xfce_2025-01-15_klatest.iso.pkgs
```

## 🔄 Workflow Architecture

### 1. Setup Phase
- **Environment preparation** and dependency installation
- **Telegram notification** with build parameters
- **Repository validation** and branch verification

### 2. Build Phase  
- **Dynamic content discovery** from ISO profiles repository
- **Package repository configuration** based on branches
- **Kernel configuration** and optimization
- **ISO compilation** with manjaro-tools integration

### 3. Release Phase
- **Artifact compression** and packaging
- **GitHub release creation** with automatic versioning
- **Progress notifications** and completion status

## 🔧 Technical Implementation

### Core Components

1. **GitHub Action** (`action.yml`):
   - Defines reusable build action
   - Handles environment setup and validation
   - Manages terminal utilities and logging

2. **Workflow** (`edition.yml`):
   - Orchestrates complete build pipeline
   - Manages Telegram notifications
   - Handles artifact creation and release

3. **Build Script** (`build-iso.sh`):
   - Core ISO building functionality
   - Repository configuration and management
   - Package optimization and cleanup

### Key Technologies
- **GitHub Actions**: Workflow automation platform
- **Manjaro Tools**: Core ISO building framework
- **Docker Container**: Isolated build environment (`talesam/community-build:1.7.1`)
- **Telegram API**: Real-time notification system

## 📊 Monitoring & Notifications

### Telegram Integration
The system provides comprehensive build monitoring through Telegram:

- **🚀 Build Start**: Parameters and configuration summary
- **⏳ Progress Updates**: Current build phase and timing
- **✅ Success Notification**: Completion time, file size, download links
- **🚨 Failure Alerts**: Error details and troubleshooting links

### GitHub Actions Logs
Detailed build logs are available in the GitHub Actions interface, including:
- Step-by-step execution tracking
- Terminal output with color coding
- Resource usage and timing information
- Artifact generation and upload status

## 🐛 Debugging & Troubleshooting

### tmate Debugging Session
Enable interactive debugging by setting the tmate option:

```bash
build-iso -o your-org --auto -t  # Enable tmate session
```

This provides SSH access to the build environment during execution.

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Profile not found | Invalid build_dir or edition | Verify against repository API |
| Authentication failed | Invalid GitHub token | Check token permissions and expiry |
| Package conflicts | Repository configuration | Review branch compatibility |
| Build timeout | Large ISO or slow network | Increase GitHub Actions timeout |

## 🤝 Contributing

We welcome contributions to improve the ISO Builder system:

1. **Fork** the repository
2. **Create** a feature branch
3. **Implement** your improvements
4. **Test** thoroughly with different configurations
5. **Submit** a pull request with detailed description

### Development Guidelines
- Maintain backward compatibility
- Add comprehensive error handling
- Update documentation for new features
- Test with multiple organizations and configurations

## 📄 License

This project is licensed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for complete terms and conditions.

## 🔗 Resources

### Official Links
- **[BigCommunity Website](https://communitybig.org)**: Main project website
- **[BigLinux Website](https://www.biglinux.com.br)**: BigLinux distribution homepage
- **[GitRepo Package](https://github.com/big-comm/gitrepo)**: Required command-line tools

### Repository Links
- **[ISO Profiles - BigCommunity](https://github.com/big-comm/iso-profiles)**: BigCommunity ISO configuration profiles
- **[ISO Profiles - BigLinux](https://github.com/biglinux/iso-profiles)**: BigLinux ISO configuration profiles
- **[Build Container](https://hub.docker.com/r/talesam/community-build)**: Docker image for build environment

### Documentation
- **[GitHub Actions Documentation](https://docs.github.com/en/actions)**: GitHub Actions reference
- **[Manjaro Tools Guide](https://wiki.manjaro.org/index.php/Manjaro-tools)**: Core build tools documentation
- **[Telegram Bot API](https://core.telegram.org/bots/api)**: Notification system API reference

---

<div align="center">
  <p><strong>Built with ❤️ by the BigCommunity Team</strong></p>
  <p>Empowering custom Linux distribution creation through automation</p>
</div>
