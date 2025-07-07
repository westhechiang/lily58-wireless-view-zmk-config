# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr-based Mechanical Keyboard) firmware configuration repository for a Lily58 split keyboard with Nice!View displays. The keyboard uses Nice!Nano v2 controllers and is configured for wireless operation via Bluetooth.

## Key Components

1. **build.yaml** - Defines the GitHub Actions build matrix for compiling firmware for both halves of the keyboard
2. **config/lily58.conf** - Main configuration file for keyboard behavior, power settings, and features
3. **config/lily58.keymap** - Defines the keyboard layout and layer configurations
4. **config/west.yml** - West manifest file that pulls in the ZMK firmware

## Build Commands

This project uses GitHub Actions for building. To build locally:
```bash
# Install west if not already installed
pip3 install --user -U west

# Initialize the workspace
west init -l config
west update

# Build for left half
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD="lily58_left nice_view_adapter nice_view" -DZMK_CONFIG="${PWD}/config"

# Build for right half  
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD="lily58_right nice_view_adapter nice_view" -DZMK_CONFIG="${PWD}/config"
```

## Architecture

The keyboard firmware is built on ZMK, which uses:
- **Device Tree** format for keymap configuration (.keymap files)
- **Kconfig** for feature configuration (.conf files)
- **West** for dependency management and building
- **GitHub Actions** for CI/CD builds

## Key Features Configured

- **Nice!View displays** on both keyboard halves for status information
- **Bluetooth** with increased transmission power for better range
- **ZMK Studio** enabled for real-time keymap updates
- **Volume control** via rotary encoders
- **3 layers**: Base, Lower, and Raise with function keys and symbols
- **Reserved layers** (3 additional) for future expansion

## Common Development Tasks

### Modifying Keymaps
Edit `config/lily58.keymap`. The keymap uses a matrix layout where each key position is defined. Layers are defined as separate blocks within the keymap.

### Adjusting Power Settings
Edit `config/lily58.conf`:
- Enable deep sleep: Uncomment `CONFIG_ZMK_SLEEP=y`
- USB logging for debugging: Uncomment `CONFIG_ZMK_USB_LOGGING=y` (increases power usage)

### Adding New Features
Most ZMK features can be enabled by adding the appropriate `CONFIG_` flags to `lily58.conf`.

## Testing Changes

Push changes to GitHub to trigger automatic builds via GitHub Actions. The workflow will build firmware for both keyboard halves and make the `.uf2` files available as artifacts.