# Intel RealSense

To use any Intel RealSense camera with an IQRLab workstation, these specific instructions to install the librealsense2 SDK must be followed since the camera requires a patched kernel module to function properly.

As of the time of writing, kernel patching Ubuntu 22.04 (Kernel 6.32) has a few hoops that need to be jumped through.

# Assumptions

Familiarity with the provided [RealSense SDK Manual Linux Installation Instructions](https://github.com/IntelRealSense/librealsense/blob/development/doc/installation.md).

# Install librealsense2 SDK

1. [Install all dependencies for Ubuntu 22.04.](https://dev.intelrealsense.com/docs/compiling-librealsense-for-linux-ubuntu-guide#install-dependencies)
2. Clone the [librealsense](https://github.com/IntelRealSense/librealsense) repo.
   ```sh
   git clone https://github.com/IntelRealSense/librealsense.git
   ```
3. Checkout the `development` branch.
   ```sh
   cd librealsense
   git checkout development
   ```
4. Run Intel Realsense permissions script from `librealsense2` root directory:
   ```sh
   ./scripts/setup_udev_rules.sh
   ```
5. Reboot the system into the UEFI:
   ```sh
   systemctl reboot --firmware-setup
   ```
6. Disable Secure Boot in the UEFI settings.
7. Build and apply patched kernel modules:
   ```sh
   ./scripts/patch-realsense-ubuntu-lts-hwe.sh
   ```
8. [Build the librealsense2 SDK](https://dev.intelrealsense.com/docs/compiling-librealsense-for-linux-ubuntu-guide#building-librealsense2-sdk)
   > Use the following build command:
   >
   > ```sh
   > cmake ../ -DCMAKE_BUILD_TYPE=Release
   > ```
9. Follow the instructions for compiling and installing the binaries
   > Tip: Use `-j15` flag in `make` commands for parallel compilation

# Install librealsense2 Python Wrapper

1. There is no need to build the Python wrapper from source. Install the pre-built package with `pip`
   ```sh
   pip install pyrealsense2
   ```

# Firmware Updates

Use the `rs-fw-update` binary installed with the librealsense2 SDK to update the firmware on the camera. For latest firmware downloads, please visit [this page](https://dev.intelrealsense.com/docs/firmware-releases) and match the firmware with your specific RealSense model.

If you are starting with a new camera, it is recommended to update the firmware to the latest version before beginning development.
