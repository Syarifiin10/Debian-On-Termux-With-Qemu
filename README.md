# Install Debian on QEMU via Termux on Android

This repository provides instructions and necessary configurations to run Debian using QEMU on an Android device via the Termux environment.

## Requirements

- **Termux**: Available on the [Google Play Store](https://play.google.com/store/apps/details?id=com.termux) or [F-Droid](https://f-droid.org/id/packages/com.termux/)
- **QEMU**: Installed via Termux
- **Debian ISO**: Download from [Debian's official website](https://www.debian.org/distrib/)

## My device specifications 

- **Android Device**: Poco X3 NFC (or any Android device with similar or better specifications)
- **RAM**: 8 GB
- **Storage**: 128 GB
- **Processor**: Snapdragon 732G (2,30 GHz)
- **GPU**: Adreno 618

## Setup Instructions

### Step 1: Install Termux

Download and install Termux from the Google Play Store.

### Step 2: Install QEMU

Open Termux and execute the following commands to update packages and install QEMU:

```sh
pkg update
pkg upgrade
pkg install qemu-system-x86_64
```

### Step 3: Prepare Disk Image and ISO

1. Download the Debian ISO (e.g., `debian-xx.x.x-amd64-netinst.iso`) from the official Debian website.
2. Create a disk image or use an existing one (e.g., `debian.img`).

Move these files to a directory accessible by Termux, such as `/storage/emulated/0/Download/`.

### Step 4: Running QEMU

#### Boot from ISO for Installation

Use the following command to start QEMU and boot from the Debian ISO for installation:

```sh
qemu-system-x86_64 \
  -M pc \
  -cpu qemu64,+avx \
  -accel tcg,thread=multi \
  -smp 2 \
  -m 2048 \
  -vga std \
  -netdev user,id=usernet \
  -device e1000,netdev=usernet \
  -hda /storage/emulated/0/Download/debian.img \
  -cdrom /storage/emulated/0/Download/debian-xx.x.x-amd64-netinst.iso \
  -boot d
```

#### Boot from Disk Image

If Debian is already installed on the disk image, use the following command to start QEMU:

```sh
qemu-system-x86_64 \
  -M pc \
  -cpu qemu64,+avx \
  -accel tcg,thread=multi \
  -smp 2 \
  -m 2048 \
  -vga std \
  -netdev user,id=usernet \
  -device e1000,netdev=usernet \
  -hda /storage/emulated/0/Download/debian.img
```

### Optional: Accessing via VNC

To access the virtual machine via VNC, use the following command:

```sh
qemu-system-x86_64 \
  -M pc \
  -cpu qemu64,+avx \
  -accel tcg,thread=multi \
  -smp 2 \
  -m 2048 \
  -netdev user,id=usernet \
  -device e1000,netdev=usernet \
  -hda /storage/emulated/0/Download/debian.img \
  -vnc :0
```

Then, connect using a VNC viewer app on your Android device.

## Troubleshooting

- **Performance Issues**: Try reducing the number of CPU cores (`-smp 1`) or RAM (`-m 1024`).
- **Storage Issues**: Ensure you have enough storage space available for the disk image and ISO.
- **Network Issues**: Make sure your network configuration is correct and the `user` mode is used for simplicity.

## Contributing

Feel free to open issues or submit pull requests for improvements and fixes.

## License

This project is licensed under the MIT License.

*Click [here](https://github.com/Syarifiin10/Debian-On-Termux-With-Qemu/blob/main/LICENSE) to view license*
