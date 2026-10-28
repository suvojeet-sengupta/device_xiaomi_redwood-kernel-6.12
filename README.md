# device_xiaomi_redwood-kernel-6.12

Prebuilt **6.12.81-Suvkernel** for redwood (POCO X5 Pro 5G / Redmi Note 12 Pro 5G, SM7325):
Qualcomm KERNEL.PLATFORM.5.0.r48-00400 (android16-6.12) with the redwood port from
[redwood-6.12-kernel](https://github.com/suvojeet-sengupta/redwood-6.12-kernel) (branch `r48`).

| Path | What |
|---|---|
| `Image` | kernel |
| `dtb/` | Yupik base DTB (name kept for BoardConfig) |
| `dtbo.img` | redwood overlay |
| `vendor_ramdisk/` | every module plus `modules.load` (first stage init loads them in that order) |
| `kernel-headers/` | the 5.4 headers the ROM userspace is built against (unchanged) |

Device tree side: `BOARD_VENDOR_RAMDISK_KERNEL_MODULES` from `vendor_ramdisk/*.ko`,
`..._LOAD` from `vendor_ramdisk/modules.load`, `BOARD_DO_NOT_STRIP_VENDOR_RAMDISK_MODULES := true`
(the GKI modules for Wi-Fi and Bluetooth must keep their signatures) and
`clk_ignore_unused pd_ignore_unused` on the kernel command line.

Built by `scripts/make-prebuilt.sh` in [redwood-6.12](https://github.com/suvojeet-sengupta/redwood-6.12). See `build-info.txt`.
