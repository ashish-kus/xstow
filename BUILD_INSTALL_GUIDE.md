# Installation Guide

## Prerequisites

Before building XStow, ensure you have the following dependencies installed:

- **Build tools**: `gcc`, `make`, `autotools` (autoconf, automake, libtool)
- **Development libraries**: Standard C development environment

### Ubuntu/Debian

```bash
sudo apt-get update

sudo apt-get install build-essential autotools-dev autoconf automake libtool
```

### CentOS/RHEL/Fedora

```bash
# CentOS/RHEL
sudo yum groupinstall "Development Tools"
sudo yum install autoconf automake libtool

# Fedora
sudo dnf groupinstall "Development Tools"
sudo dnf install autoconf automake libtool
```

## Build from Source

### 1. Clone the Repository

```bash
git clone https://github.com/ashish-kus/xstow
cd project
```

### 2. Generate Configure Script

If the `configure` script is not present in the repository:

```bash
autoreconf -i
```

### 3. Configure the Build

```bash
./configure
```

**Configure Options:**

- `--prefix=/usr/local` - Installation directory (default: `/usr/local`)
- `--enable-debug` - Build with debug symbols
- `--disable-shared` - Build only static libraries

### 4. Build XStow

```bash
# Standard build (recommended for most users)
make

# Mini build (lightweight version)
make mini

# Build documentation (optional)
make docu
```

### 5. Install

```bash
# Install system-wide (requires root/sudo)
sudo make install

# Or install to custom prefix
make install PREFIX=/path/to/custom/location
```

## Binary Types & Locations

After building, XStow provides different binary variants:

### Standard Build

- **Location**: `src/xstow`
- **Description**: Full-featured version with all capabilities
- **Recommended for**: Most users and production environments

### Mini Build Variants

The mini build creates two binary types:

#### Dynamic Binary (`src/xstow-mini`)

- **Linking**: Dynamically linked to system libraries
- **File size**: Smaller (~50-200KB typical)
- **Portability**: Requires compatible system libraries on target machine
- **Performance**: Faster startup, shared library benefits
- **Best for**: Systems with matching library versions

#### Static Binary (`src/xstow-mini-static`)

- **Linking**: Statically linked (all libraries embedded)
- **File size**: Larger (~500KB-2MB typical)
- **Portability**: Runs on systems without specific library versions
- **Performance**: Slightly slower startup, no library dependencies
- **Best for**: Deployment to different systems, containers, or embedded environments

### Checking Binary Dependencies

```bash
# Check dynamic dependencies
ldd src/xstow-mini

# Static binary (will show "not a dynamic executable" or minimal deps)
ldd src/xstow-mini-static
```

## Installation Options

### Option 1: System Installation (Recommended)

```bash
sudo make install
```

- Installs to `/usr/local/bin/xstow` (or configured prefix)
- Available system-wide for all users
- Follows standard Unix installation practices

### Option 2: Local Installation

```bash
# Copy to user's local bin directory
mkdir -p ~/.local/bin
cp src/xstow ~/.local/bin/
# Ensure ~/.local/bin is in your PATH
export PATH="$HOME/.local/bin:$PATH"
```

### Option 3: Portable Usage

```bash
# Use directly from build directory (no installation)
./src/xstow [options]

# For static binary (most portable)
./src/xstow-mini-static [options]
```

## Quick Start

After installation, verify XStow is working:

```bash
# Check version and help
xstow --version
xstow --help

# Basic usage example
xstow package-name
```

## Choosing the Right Binary

| Use Case                      | Recommended Binary  | Reason                           |
| ----------------------------- | ------------------- | -------------------------------- |
| Production servers            | `xstow` (standard)  | Full features, tested stability  |
| Development/testing           | `xstow-mini`        | Faster builds, adequate features |
| Docker containers             | `xstow-mini-static` | No dependency issues             |
| Embedded systems              | `xstow-mini-static` | Minimal system requirements      |
| Multi-distribution deployment | `xstow-mini-static` | Maximum compatibility            |

## Troubleshooting

### Build Errors

- **Missing autotools**: Run `autoreconf -i` after installing autotools
- **Configure fails**: Check for missing development headers
- **Make errors**: Ensure all prerequisites are installed

### Runtime Issues

- **Command not found**: Check if installation directory is in `PATH`
- **Library errors** (dynamic binary): Install missing shared libraries
- **Permission denied**: Ensure binary has execute permissions (`chmod +x`)

### Clean Build

```bash
make clean          # Clean build artifacts
make distclean      # Clean everything including configure
```
