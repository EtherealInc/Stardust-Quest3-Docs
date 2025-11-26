# Stardust-Quest3-Docs
Documentation to get started with stardust on the Quest 3


Quest 3 + Linux Stardust XR Setup (Fedora Guide)

Working stack:
🧠 Fedora Linux • 🌀 Monado OpenXR • 🌌 Stardust XR Desktop • 📡 WiVRn Wired Streaming • 🎮 Meta Quest 3

## This guide describes the exact steps needed to make Stardust XR run on a Meta Quest 3 streamed from Fedora Linux using WiVRn.

| Item                    | Notes                                             |
| ----------------------- | ------------------------------------------------- |
| Fedora 39+ / 40+ or 43+ | Tested on Fedora 43                               |
| Meta Quest 3            | Enable Developer Mode                             |
| USB Cable               | Stable, data-capable (⚠ bad cables fail silently) |
| WiFi 5 / 6              | Ethernet recommended between PC and router        |
| AMD or NVIDIA GPU       | Tested with AMD RDNA3 laptop (790M)               |

This will continue assuming you have base fedora installed 

Once Fedora loads:

Open terminal (CTRL+ALT+T) and run:
```
sudo dnf update -y
lspci | grep -i vga
sudo dnf install vulkan-tools -y
vulkaninfo | less
```

Install the proper development tools
```
sudo dnf install -y @development-tools
```
```
sudo dnf install -y \
  cmake meson ninja-build git pkg-config \
  vulkan-tools vulkan-loader-devel mesa-vulkan-drivers \
  libX11-devel libXrandr-devel libXinerama-devel libXcursor-devel \
  wayland-devel libxcb-devel libXext-devel \
  glfw-devel libusb1-devel systemd-devel \
  python3-pip
```
Clone and build Monado (OpenXR runtime)
```
cd ~
git clone https://gitlab.freedesktop.org/monado/monado.git
cd monado
./scripts/build.sh
```
```
sudo ./scripts/install.sh
```
Set Monado Active
```
sudo mkdir -p /etc/xdg/openxr/1
sudo ln -sf /usr/local/share/openxr/1/openxr_monado.json /etc/xdg/openxr/1/active_runtime.json
```

Run to double check the correct packages are installed, it's fine if they are already installed. 
```
sudo dnf install -y \
gcc gcc-c++ cmake ninja-build python3-pip git \
mesa-libGL-devel mesa-libEGL-devel mesa-libgbm-devel \
vulkan-loader vulkan-tools vulkan-validation-layers \
libX11-devel libXrandr-devel libXcursor-devel libXi-devel \
wayland-devel libxkbcommon-devel libdrm-devel \
SDL2-devel glfw-devel glm-devel

```
Additional Required Python Tools
```
pip3 install meson mako
```
Clone Stardust
```
cd ~
git clone https://github.com/StardustXR/stardust-xr.git
cd stardust-xr


curl https://sh.rustup.rs -sSf | sh   # if you haven't run this before
source ~/.cargo/env


cargo build --release
```

Quick Monado install sanity check
```
sudo dnf install \
    monado \
    monado-devel \
    openxr-devel \
    vulkan-tools \
    vulkan-loader \
    vulkan-loader-devel
```
Install Android tools
```
sudo dnf install android-tools
```
Download the latest Envision [AppIamge ](https://gitlab.com/gabmus/envision/-/pipelines?ref=main&status=success)

Open the Envision App Image and Build the WiVrn Default Profile

You may have to install a few more packages

```
sudo dnf install -y boost-devel
```
```
sudo dnf install -y librsvg2-devel
```
```
sudo dnf install -y libarchive-devel
```
### Enable Developer mode on the quest 3
Via the Meta/Horizon phone app → Menu → Devices → your Quest → Developer Mode

Plug the Quest 3 into your Fedora PC via USB-C.

Put on the headset and accept “Allow USB debugging?” → Always allow / Allow.

In Envision on the PC:

Select your WiVRn profile (bottom dropdown).

Click “Install WiVRn”.
That should push the APK to the headset (shows up later under Library → Unknown Sources).

If for some reason the install did not work, you can manually install the WiVrn-standard-release.apk from [WiVrn](https://github.com/WiVRn/WiVRn/releases)

Install manually:
```
adb install -r ~/Downloads/WiVRn-standard-release.apk
```
If all goes well you should be able to run WiVrn on the headset and click start WiVrn on desktop to pair 

In a seperate terminal run
```
telescope
```
and you should see Stardust running with HexagonLauncher








