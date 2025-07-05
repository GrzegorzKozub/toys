# Toys

Builds

## Player

Box

- AMD Ryzen 9 9950X3D
- Asus ROG Strix B850-I Gaming (BIOS 1063 AGESA 1.2.0.3e)
- Asus TUF Gaming GeForce RTX 5090 32GB (silent BIOS)
- Corsair SF1000
- G.Skill Flare X5 64GB 6000MHz DDR5 CL30
- Jonsbo D32 Pro
- Noctua NF-A12x15 PWM chromax.black.swap (x3)
  - [ ] Noctua NF-A12x15 PWM (x3)
- Noctua NF-A12x25 PWM chromax.black.swap (x2)
  - [ ] Noctua NF-A12x25 G2 PWM (x2)
- Noctua NH-U12A chromax.black
  - [ ] Noctua NH-D15 G2
- Samsung 980 Pro 1TB (PCIe x4 16 GT/s @ x4 16 GT/s)
- Samsung 9100 Pro 4TB (PCIe x4 32 GT/s @ x4 32 GT/s)

Monitors

- LG 27GP950-B
  - [ ] Asus PG32UCDMR ~PG32UCDM~
  - [ ] MSI MPG 322URX ~321URX~

Peripherals

- Glorious PC Gaming Race Model O 2 Wireless
- GuliKit KingKong 2 Pro NS09
- Hagibis MC40 / Qwiizlab ES40UR
- KBDfans D84 v2
- Vortex Race 3

Audio

- FiiO K11 R2R
- Sennheiser HD 58X Jubilee

### Airflow

- CPU cooler rear intake NH-U12A with NF-A12x25 (x2)
- Bottom case fans intake NF-A12x15 (x3)
- GPU fans bottom intake
- Top case fans exhaust NF-A12x25 (x2)

### Firmware settings

- Ai Tweaker
  - Ai Overclock Tuner: EXPO I
  - Precision Boost Overdrive
    - Medium Load Boostit: Enabled
  - Core tunings Configuration for gaming: Level 2
- Advanced
  - SATA Configuration
    - SATA Controller(s): Disabled
  - Onboard Devices Configuration
    - Wi-Fi Controller: Disabled
    - LED Lighting
      - When system is in working state: Stealth Mode
  - NB Configuration
    - Integrated Graphics: Disabled
  - AMD CBS
    - Global C-state Control: Enabled
  - AMD Overclocking
    - Precision Boost Overdrive
      - Precision Boost Overdrive: Advanced
      - PBO Limits: Motherboard
      - Precision Boost Overdrive Scalar Ctrl: Manual
      - Precision Boost Overdrive Scalar: 10X
      - CPU Boost Clock Override: Enabled (Positive)
      - Max CPU Boost Clock Override(+): 50
      - Curve Optimizer
        - Curve Optimizer: Per CCD
        - CCD 0 Curve Optimizer Sign: Negative
        - CCD 0 Curve Optimizer Magnitude: 25
        - CCD 1 Curve Optimizer Sign: Negative
        - CCD 1 Curve Optimizer Magnitude: 20
    - SoC/Uncore OC Mode: Enabled
- Boot
  - Boot Logo Display: Disabled
  - POST Report: 1 sec
- Tool
  - Full HD Setup: Enabled
  - ASUS MyHotkey
    - Hotkey F3: Boot from UEFI USB
    - Hotkey F4: Disabled
  - ASUS DriverHub
    - Download & Install ASUS DriverHub app: Disabled
- Q-Fan
  - CPU Fan
    - Curve: 20°C 20%, 45°C 50%, 60°C 50%, 70°C 60%, 80°C 100%
  - Chasis Gan
    - Q-Fan Source: CPU
    - Curve: 20°C 20%, 45°C 50%, 60°C 50%, 70°C 60%, 80°C 100% (same as CPU)
  - Extra Flow Fan
    - Q-Fan Source: CPU
    - Curve: 20°C 20%, 45°C 50%, 60°C 50%, 70°C 60%, 80°C 100% (same as CPU)

Settings saved with BIOS version 1028 to `player-bios.cmo` and `player-bios.txt` files.

Links

- [Motherboard firmware](https://rog.asus.com/motherboards/rog-strix/rog-strix-b850-i-gaming-wifi/helpdesk_bios/)
- [Core tunings Configuration for gaming](https://www.reddit.com/r/Amd/comments/1h8siwi/comment/m0xt2nt/)

### CPU

Benchmarks with Cinebench 2024.1.0 (pts): multi core 2427, single core 139

Temperatures (°C): idle 54, gaming 64 @ 5%, stress 82

Links

- [CPU overclocking & undervolting guide](https://skatterbencher.com/2025/03/11/skatterbencher-85-ryzen-9-9950x3d-overclocked-to-5900-mhz/)
- [AM5 Optimization Guide](https://www.techpowerup.com/forums/threads/guide-amd-am5-system-optimization.330322/)

### GPU

*RTX 5090* is undervolted by applying a 1000 MHz positive offset to the frequency at the desired voltage and flattening the curve from there. Additionally, power limit can be applied.

In Afterburner curve editor, the frequency was increased by 1000 MHz for the desired voltage. Apply was hit. All points above the target frequency were selected while holding `Shift`. Any of the selected points was dragged down so that all of them dropped below the target frequency. Apply was hit again.

Profiles and results

1. Stock: 2680 MHz, 1005 mV, 600 W, GPU 74 °C, CPU 63 °C, 14333 pts
2. 2917 Mhz @ 900 mV: 2512 MHz, 895 mV, 525 W, GPU 71 °C, CPU 63 °C, 13964 pts
3. 2572 Mhz @ 875 mV: 2310 MHz, 870 mV, 445 W, GPU 68 °C, CPU 63 °C, 13028 pts
4. 2242 Mhz @ 850 mV: 2025 MHz, 845 mV, 365 W, GPU 63 °C, CPU 62 °C, 11647 pts (applied on startup)

Benchmarked with 3DMark 2.31.8385 Steel Nomad on Windows 11 24H2 26100.4484 and NVIDIA 576.80 with vertical sync was disabled.

Temperatures (°C): idle 39, gaming 58 @ 54%

Links

- [Afterburner 4.6.6 Beta 5 for RTX 5000 series](https://forums.guru3d.com/threads/msi-afterburner-4-6-6-beta-5-for-nvidia-geforce-rtx-5000-series-cards.455155/)
- [5090FE Undervolt guide - better than stock at 450w](https://www.reddit.com/r/nvidia/comments/1jaz2yq/5090fe_undervolt_guide_better_than_stock_at_450w/)
- [RTX 5090 FE undervolt results](https://www.reddit.com/r/nvidia/comments/1isi8ir/rtx_5090_fe_undervolt_results/)
- [TUF Gaming 5090 Undervolt/Overclock Guide/Results](https://www.reddit.com/r/overclocking/comments/1jjlyix/tuf_gaming_5090_undervoltoverclock_guideresults/)

### Disks

- 4 TB
  - 4 GB EFI
  - 256 GB Windows
  - 776 GB
    - 8 GB swap
    - 256 GB `/`
    - 512 GB `/run/media/greg/data`
  - 1 TB Data
  - 1 TB Games
  - 641 GB `/run/media/greg/games`
- 932 GB Backup

Benchmarks with CrystalDiskMark 9.0.1 (MB/s)

- Samsung 9100 Pro 4TB
  - SEQ1M Q8T1: read 14698, write 13453
  - SEQ1M Q1T1: read 9580, write 10340
  - RND4K Q32T1: read 865, write 743
  - RND4K Q1T1: read 82, write 250
- Samsung 980 Pro 1TB
  - SEQ1M Q8T1: read 6876, write 4950
  - SEQ1M Q1T1: read 4184, write 4265
  - RND4K Q32T1: read 877, write 741
  - RND4K Q1T1: read 79, write 272

Temperatures (°C)

- Samsung 9100 Pro 4TB
  - idle: NAND 47, controller 53
  - stress: NAND 57, controller 64
- Samsung 980 Pro 1TB
  - idle: NAND 51, controller 61
  - stress: NAND 75, controller 93

### Monitors

*LG 27GP950-B* was calibrated for *Gamer 1* profile, brightness set to 10 (about 130 nits) and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 50 47. Software calibration is stored in the ICC profile `27gp950-b.10.icm`. Not relevant for calibration, contrast looks best at 70.

TFT Central provided `27gp950-b.6.icm` ICC profile was created for *Gamer 1* profile, brightness set to 6, RGB settings at 50 48 45 and the contrast of 70.

*Asus PG32UCDM* settings:

- Gaming
  - Game Visual: User
- Image
  - Brightness: 42 for 120 nits, 55 for 150 nits, 75 for 200 nits or 100 for 256 nits
  - Uniform Brightness: Enabled
  - Contrast: 70
  - HDR Setting: Gaming HDR or Console HDR
- Color
  - Display Color Space: Wide Gamut (vivid) or sRGB (clamped)
  - Color Temp: User with RGB at 98 100 100 when Wide Gamut or 6500K when sRGB
  - Gamma: 2.2

TFT Central provided `pg32ucdm.42.icm` ICC profile was created for brightness set to 42 (120 nits), RGB settings at 98 100 100 and uniform brightness enabled.

[Monitor firmware](https://rog.asus.com/monitors/27-to-31-5-inches/rog-swift-oled-pg32ucdm/helpdesk_bios/)

### Keyboards

For *KBDfans D84 v2*, use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `d84v2-0.png` and `d84v2-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt+PrintScreen`.

## Worker

Box

- AMD Ryzen 9 5900X
- Asus TUF Gaming OC GeForce RTX 3080 10GB (silent BIOS)
- Cooler Master NR200P
- Corsair SF750
- G.Skill Trident Z Neo 32GB 3600MHz DDR4 CL16
- Gigabyte B550I Aorus Pro AX 1.0 (BIOS F19 AGESA 1.2.0.B)
- Noctua NF-A12x15 PWM (x2)
- Noctua NF-A12x25 PWM (x2)
- Noctua NH-U12A
- Samsung 980 Pro 1TB (PCIe x4 16 GT/s @ x4 8 GT/s)
- Samsung 990 Pro 2TB (PCIe x4 16 GT/s @ x4 16 GT/s)

Monitors

- LG 27UD88-W
- LG 27UL850-W

Peripherals

- Glorious PC Gaming Race Model O Wireless
- Idobao ID80V3
- Logitech C922 Pro Stream Webcam
- Vortex Race 3
- Zowie EC1-A

### Airflow

- CPU cooler rear exhaust NH-U12A with NF-A12x25 (x2)
- Bottom case fans intake NF-A12x15 (x2)
- GPU fans bottom intake
- Top case fans exhaust NF-A12x25 (x2)

### Firmware settings

- Tweaker
  - CPU Clock Ratio: 42.00
  - Advanced CPU Settings
    - SVM Mode: Enabled
    - Global C-state Control: Enabled
  - Extreme Memory Profile(X.M.P.): Profile 1
  - CPU Vcore: 1.028V
- Settings
  - Platform Power
    - Wake on LAN: Disabled
  - IO Ports
    - Above 4G Decoding: Enabled
    - Re-Size BAR Support: Auto
    - APP Center Download & Install Configuration
      - APP Center Download & Install: Disabled
    - SATA Configuration
      - Chipset SATA Port Enable: Disabled
      - Chipset SATA Port Hot Plug: Disabled
  - Miscellaneous
    - LEDs in System Power On State: Off
    - AMD CPU fTPM: Enabled
  - AMD CBS
    - CPU Common Options
      - Global C-state Control: Enabled
  - AMD Overclocking
    - SOC/Uncore OC Mode: Enabled
- Smart Fan 5
  - CPU_FAN
    - Curve: 0°C 0%, 40°C 40%, 55°C 50%, 60°C 60%, 70°C 80%, 80°C 100%
    - CPU_FAN Speed Control: Manual
    - Temperature Interval: 3
  - SYS_FAN1
    - Curve: 0°C 0%, 40°C 40%, 55°C 50%, 60°C 60%, 70°C 80%, 80°C 100%
    - SYS_FAN1 Speed Control: Manual
    - Fan Control Use Temperature Input: System 1
    - Temperature Interval: 3
  - SYS_FAN2
    - Curve: 0°C 0%, 50°C 35%, 60°C 40%, 70°C 60%, 75°C 80%, 80°C 100%
    - SYS_FAN2 Speed Control: Manual
    - Fan Control Use Temperature Input: System 1
    - Temperature Interval: 3
- Boot
  - Full Screen LOGO Show: Disabled
  - CSM Support: Disabled
  - Secure Boot
    - System Mode: User
    - Secure Boot: Enabled, Active
    - Secure Boot Mode: Standard

[Motherboard firmware](https://www.gigabyte.com/Motherboard/B550I-AORUS-PRO-AX-10/support#dl)

### CPU

Benchmarks with Cinebench 2024.1.0 (pts): multi core 1195, single core 86

Temperatures (°C): idle 33, gaming 45 @ 15%, stress 55

### GPU

*RTX 3080* has 1440 MHz base clock and 1710 MHz boost clock. The default frequency curve allows up to 2000 MHz. At stock, when gaming with 60 FPS cap, the clock reaches 1920 MHz at 1056 mV.

In Afterburner, the core clock was set to -290 MHz (2000 - 1710). In curve editor, the frequency was set to 1890 Mhz at 912 mV. The apply button was hit.

Profiles and results

1. Stock: ...
2. 1890 Mhz @ 912 mV: ..., GPU 70 °C, CPU 42 °C, 4504 pts (applied on startup)

Benchmarked with 3DMark 2.31.8385 Steel Nomad on Windows 11 24H2 26100.4351 and NVIDIA 576.40 with vertical sync was disabled.

Temperatures (°C): idle 28, gaming 65 @ 85%

### Disks

- 2 TB
  - 4 GB EFI
  - 128 GB Windows
  - 776 GB
    - 8 GB swap
    - 256 GB `/`
    - 512 GB `/run/media/greg/data`
  - 476 GB Data
  - 476 GB Games
- 1 TB
  - 466 GB Backup
  - 466 GB `/run/media/greg/games`

Benchmarks with CrystalDiskMark 9.0.1 (MB/s)

- Samsung 990 Pro 2TB
  - SEQ1M Q8T1: read 7409, write 6505
  - SEQ1M Q1T1: read 3337, write 5551
  - RND4K Q32T1: read 470, write 289
  - RND4K Q1T1: read 59, write 175
- Samsung 980 Pro 1TB
  - SEQ1M Q8T1: read 3251, write 3164
  - SEQ1M Q1T1: read 2639, write 2812
  - RND4K Q32T1: read 541, write 509
  - RND4K Q1T1: read 77, write 162

Temperatures (°C)

- Samsung 990 Pro 2TB
  - idle: NAND 36, controller 41
  - stress: NAND 51, controller 62
- Samsung 980 Pro 1TB
  - idle: NAND ..., controller ...
  - stress: NAND ..., controller ...

### Monitors

LG *27UL850-W* was calibrated for *Custom* profile, brightness set to 30 and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 49 50. Software calibration is stored in the ICC profile `27ul850-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ul850-w.50.icm`.

LG *27UD88-W* was calibrated for *Custom* profile, brightness set to 30 and gamma mode 1 selected. Hardware calibration resulted in the RGB settings of 49 47 50. Software calibration is stored in the ICC profile `27ud88-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ud88-w.50.icm`.

### Keyboards

For *Idobao ID80V3* and *ID80V2* follow [this guide](https://idobao.github.io/manuals/flashing/) to update the firmware and use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `id80v3-0.png` and `id80v3-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt+Fn+F10`.

## Sacrifice

Box

- Asus Dual OC V2 Radeon RX 6650 XT 8GB
- Asus ROG Strix Z370-I Gaming (BIOS 3005)
- Corsair SF600
- Fractal Design Define Nano S
- HyperX Fury Black 16GB 2133MHz DDR4 CL14
- Intel Core i9-9900K
- Noctua NH-U9S
- Samsung 840 Evo 500GB (SATA 6 Gb/s)
- Samsung 970 Evo 1TB (PCIe x4 8 GT/s @ x4 8 GT/s)

### Firmware settings

- Ai Overclock Tuner: XMP
- CPU Core Ratio: Sync All Cores
- 1-Core Ratio Limit: 44
- CPU Core/Cache Voltage: Manual Mode
- CPU Core Voltage Override: 1.200
- Security Device Support: Enabled
- Intel Virtualization Technology: Enabled
- SATA6G_1(Charcoal Black): Disabled
- SATA6G_3(Charcoal Black): Disabled
- SATA6G_4(Charcoal Black): Disabled
- Above 4G Decoding: Enabled
- Re-Size BAR Support: Auto
- Wi-Fi Controller: Disabled
- CPU Fan Profile: Manual
- CPU Upper Temperature: 75
- CPU Fan Max. Duty Cycle (%): 100
- CPU Middle Temperature: 70
- CPU Fan Middle. Duty Cycle (%): 80
- CPU Lower Temperature: 50
- CPU Fan Min. Duty Cycle (%): 60
- Chassis Fan Profile: Manual
- Chassis Fan Upper Temperature: 75
- Chassis Fan Max. Duty Cycle (%): 100
- Chassis Fan Middle Temperature: 70
- Chassis Fan Middle. Duty Cycle (%): 80
- Chassis Fan Lower Temperature: 50
- Chassis Fan Min. Duty Cycle (%): 60
- Boot Logo Display: Disabled
- POST Report: 1 sec
- Profile Name: 4.4GHz@1.2V

Settings saved with BIOS version 3005 to `sacrifice-bios.cmo` and `sacrifice-bios.txt` files.

Enabled SGX using [sgx-software-enable](https://github.com/intel/sgx-software-enable).

*Security Device Support* is required for Windows 11 but when it is enabled, `dmesg` reports `tpm_crb MSFT0101:00: [Firmware Bug]: ACPI region does not cover the entire command/response buffer.`

*Above 4G Decoding* causes a black screen when trying to run Reflect or Windows setup.

[Motherboard firmware](https://rog.asus.com/motherboards/rog-strix/rog-strix-z370-i-gaming-model/helpdesk_bios/)

### Disks

- 1 TB
  - 4 GB EFI
  - 128 GB Windows
  - 392 GB
    - 8 GB swap
    - 128 GB `/`
    - 256 GB `/run/media/greg/data`
  - 406 GB Data
- 465 GB Backup

