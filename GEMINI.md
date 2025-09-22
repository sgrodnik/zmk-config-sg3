# ZMK Firmware Configuration (sg3)

This repository contains the ZMK firmware configuration for the "sg3" split keyboard. It leverages the Zephyr RTOS and ZMK firmware to provide a highly customizable keyboard experience, including custom keymaps, macros, and hardware definitions.

## Project Overview

This project is a ZMK user configuration, designed to be built against the main ZMK firmware repository. It defines the keymap, behaviors, and hardware-specific settings for a split keyboard utilizing `nice_nano_v2` boards and `sg3_left`/`sg3_right` shields.

Key features and configurations include:
*   **Split Keyboard Support**: Configured for left and right halves of a split keyboard.
*   **Rotary Encoder Support**: Includes configuration for an EC11 rotary encoder.
*   **Pointing Device Support**: Enabled for mouse functionality.
*   **Custom Keymap (`sg3.keymap`)**: Defines multiple layers for alphanumeric input, numbers/function keys, navigation, browser/system controls, and mouse movement. It includes extensive custom macros for common actions (e.g., copy/paste, task manager, Alt-codes) and hold-tap behaviors.
*   **Combos**: Numerous key combinations are defined for efficient input of symbols, Bluetooth device selection, and mouse clicks.
*   **West Workspace**: Uses `west` (Zephyr's meta-tool) to manage dependencies, pulling in the main ZMK firmware and additional ZMK modules like `zmk-rgbled-widget` and `zmk-auto-layer`.

## Building and Running

The project is designed to be built using the `west` tool, which is part of the Zephyr RTOS development environment.

### GitHub Actions

The `.github/workflows/build.yml` file defines a GitHub Actions workflow that automatically builds the firmware for `sg3_left` and `sg3_right` shields using a shared ZMK build workflow. This ensures that firmware binaries are generated on every push and pull request.

### Local Building

To build the firmware locally, you will need to set up the Zephyr development environment and `west`.

1.  **Initialize West Workspace**:
    ```bash
    west init -l config
    west update
    ```
2.  **Build the Firmware**:
    The `build.yaml` file specifies the build matrix. You can build individual shields using `west build`. For example:
    ```bash
    west build -s zmk/app -b nice_nano_v2 -- -DSHIELD=sg3_left -DCONFIG_ZMK_STUDIO=y
    west build -s zmk/app -b nice_nano_v2 -- -DSHIELD=sg3_right -DCONFIG_ZMK_STUDIO=y
    ```
    The `-DCONFIG_ZMK_STUDIO=y` flag is included as per the `build.yaml` snippet, indicating potential integration with ZMK Studio.

## Development Conventions

*   **ZMK Firmware Structure**: Adheres to the standard ZMK user configuration structure, with keymaps, configuration, and board/shield definitions located under the `config/` directory.
*   **Device Tree Overlays (`.dtsi`, `.overlay`)**: Hardware definitions and modifications are managed using Device Tree Source Include files and overlays.
*   **Kconfig**: Project-specific and shield-specific configurations are defined using Kconfig files (`.conf`, `Kconfig.shield`).
*   **Macros and Behaviors**: Extensive use of ZMK's macro and behavior system for complex key actions and layer management.
*   **West Manifest (`west.yml`)**: Manages external ZMK modules and the main ZMK firmware as dependencies.
