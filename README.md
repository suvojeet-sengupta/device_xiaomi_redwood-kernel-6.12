# device_xiaomi_redwood-kernel-6.12

A ready-to-use **Linux 6.12** kernel for the **Redmi Note 12 Pro 5G / POCO
X5 Pro 5G** (`redwood`, SM7325), for building a ROM. Nothing here has to be
compiled: the ROM build just copies these files in.

The kernel is **Suvkernel 2.0** (`6.12.97-Suvkernel-2.0`, Linux 6.12.97), built from the `main` branch of
[redwood-6.12-kernel](https://github.com/suvojeet-sengupta/redwood-6.12-kernel).
`build-info.txt` says exactly which build this is.

## What's inside

| File | What it is |
|---|---|
| `Image` | The kernel |
| `dtb/` | The SoC device tree (the file name is kept so the device tree finds it) |
| `dtbo.img` | redwood's own hardware description, laid over the SoC one |
| `vendor_ramdisk/` | Every kernel module, plus `modules.load`, the order they load in |
| `kernel-headers/` | The 5.4 kernel headers. On purpose: the phone's vendor userspace was built against them |
| `build-info.txt` | Kernel version, build date and module count |

## How to use it

Use the `clo-6.12` branch of the redwood device tree. It already points
here, and the `clo-6.12-setup` branch has a manifest that syncs everything
for you.

Any other redwood device tree, AOSP or AOSPA, works too, even one made for
the stock 5.4 kernel: this kernel understands the 5.4-era `post_boot`
script, sysfs paths, Wi-Fi config and media script those trees ship. Wire
it in with:

```make
KERNEL_PATH := device/xiaomi/redwood-kernel-6.12
TARGET_PREBUILT_KERNEL := $(KERNEL_PATH)/Image
TARGET_BOARD_KERNEL_HEADERS := $(KERNEL_PATH)/kernel-headers
BOARD_PREBUILT_DTBOIMAGE := $(KERNEL_PATH)/dtbo.img
BOARD_PREBUILT_DTBIMAGE_DIR := $(KERNEL_PATH)/dtb
BOARD_VENDOR_RAMDISK_KERNEL_MODULES := $(wildcard $(KERNEL_PATH)/vendor_ramdisk/*.ko)
BOARD_VENDOR_RAMDISK_KERNEL_MODULES_LOAD := $(strip $(shell cat $(KERNEL_PATH)/vendor_ramdisk/modules.load))
# Wi-Fi and Bluetooth use signed GKI modules: stripping breaks the signature
BOARD_DO_NOT_STRIP_VENDOR_RAMDISK_MODULES := true
```

and drop the 5.4 kernel's own module lists and `vendor_dlkm` modules, if
the tree has any. If the ROM builds with Dolby, which sets
`debug.c2.use_dmabufheaps=1`, set it back to `0` (for example `on
early-boot` in an init script): otherwise videos don't play.

## Before you flash

- This is a different kernel from the stock 5.4 one, and `/data` is shared
  between the two slots. Back up your data before trying it.
- Don't mix it with a 5.4 boot or vendor_boot image: the modules only load
  on this exact kernel build.

## Updating

The files are made by `scripts/make-prebuilt.sh` in
[redwood-6.12](https://github.com/suvojeet-sengupta/redwood-6.12) after
every kernel change. Each commit here says which kernel commit its files
came from.
