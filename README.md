# bldc-bootloader

## GigaDevice GD32F405 builds (branch gd32)

For VESC boards where the STM32F405RGT6 is replaced by a pin compatible
GD32F405. Must be used together with firmware from the `gd32` branch of
[spencer1979/bldc](https://github.com/spencer1979/bldc/tree/gd32), see
`documentation/gd32f405_port.md` there.

| MCU | Build | Bootloader location | New app staging | App sectors | Firmware target |
|---|---|---|---|---|---|
| STM32F405RG (stock) | `make build_args='-DHW_VER=60'` | 0x080E0000 (s11) | 0x08080000 (s8-s10, 384 KB) | s0-s7 | `fw_60` |
| GD32F405RK (3 MB) | `make GD32=1 build_args='-DHW_VER=60'` | 0x08100000 (s12) | 0x08200000 (s24-s27, 1 MB) | s0-s11 | `fw_60_gd32` |
| GD32F405RG (1 MB) | `make GD32_1M=1 build_args='-DHW_VER=60'` | 0x080E0000 (s11) | 0x08080000 (s8-s10, 384 KB) | s0-s7 | `fw_60_gd32_1m` |

* `GD32=1` and `GD32_1M=1` both define `GD32F405_COMPAT` (GD32 flash wait
  state register: only WSCNT is written; safe PLL -> IRC16M switch with
  AHB/4 first). `GD32=1` also defines `GD32F405_FLASH_3M` (3 MB sector
  table, linker script `ld_bootloader_gd32.ld`).
* The GD32F405RG keeps the stock STM32 layout, so the stock flow (VESC Tool
  firmware and bootloader upload) is unchanged.
* Run `make clean` when switching between variants.
* Flash with SWD: firmware `.bin` at 0x08000000, then this bootloader at
  the address above. Not verified on hardware yet.
