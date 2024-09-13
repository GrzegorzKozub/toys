# Toys

Hardware manifest and settings

## Next

- NCase M2
- Lian Li A3-mATX

## Player

- AMD Ryzen 9 5900X
- Asus TUF RTX 3080 10GB Gaming
- Cooler Master NR200P
- Corsair SF750 Platinum
- G.SKILL Trident Z Neo 32GB 3600MHz DDR4 CL16
- Gigabyte B550I Aorus Pro AX (BIOS F19 AGESA 1.2.0.B)
- Glorious PC Gaming Race Model O 2 Wireless
- GuliKit KingKong 2 Pro NS09
- KBDfans D84 v2
- LG 27GP950-B
- Noctua NF-A12x25 PWM
- Noctua NH-U12A
- Samsung 850 Pro 500GB
- Samsung 980 Pro 1TB
- Vortex Race 3

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
    - Curve: 0c 0%, 40c 40%, 55c 50%, 60c 60%, 70c 80%, 80c 100%
    - CPU_FAN Speed Control: Manual
    - Temperature Interval: 3
  - SYS_FAN1
    - Curve: 0c 0%, 40c 40%, 55c 50%, 60c 60%, 70c 80%, 80c 100%
    - SYS_FAN1 Speed Control: Manual
    - Fan Control Use Temperature Input: System 1
    - Temperature Interval: 3
  - SYS_FAN2
    - Curve: 0c 0%, 50c 35%, 60c 40%, 70c 60%, 75c 80%, 80c 100%
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

### GPU undervolting

In Afterburner, set core clock to -290MHz and drag 900mV point to 1920MHz. Could go 875mV but that causes games like Metro to crash.

### Monitor settings

LG 27GP950-B was calibrated for Gamer 1 profile, brightness set to 10 (about 130 nits) and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 50 47. Software calibration is stored in the ICC profile `27gp950-b.10.icm`. Not relevant for calibration, contrast looks best at 70.

TFT Central provided `27gp950-b.6.icm` ICC profile was created for Gamer 1 profile, brightness set to 6, RGB settings at 50 48 45 and the contrast of 70.

Asus PG32UCDM settings which were enabled and/or modified: 

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

PG32UCDM got returned due to aggressive VRR flickering during load screens and framerate dips when G-Sync was enabled. Mitigation options were to either disable VRR or set the refresh rate to 60 Hz before launching a game.

### Keyboard settings

For KBDfans D84 v2, use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `d84v2-0.png` and `d84v2-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt+PrintScreen`.

### Benchmarks

Setup:

- BIOS F19 AGESA 1.2.0.B
- Windows 11 23H2 22631.3447
- AMD chipset drivers not installed
- NVIDIA 552.22
- core isolation (memory integrity) off

Results:

- Cinebench 2024.0.0 (pts): multi core 1194, single core 86, GPU 12844
- 3DMark 2.29.8256 (pts)
  - Steel Nomad 1.0: score 4494
  - Time Spy Extreme 1.2: score 8810, graphics score 9028, CPU score 7752
  - Port Royal 1.3: score 11802
  - Speed Way 1.0: score 4685
- CrystalDiskMark 8.0.5 (MB/s)
  - NVME 1
    - SEQ1M Q8T1: read 6849, write 5014
    - SEQ1M Q1T1: read 4183, write 4354
    - RND4K Q32T1: read 592, write 449
    - RND4K Q1T1: read 86, write 207
  - NVME 2
    - SEQ1M Q8T1: read 3242, write 3166
    - SEQ1M Q1T1: read 2712, write 2898
    - RND4K Q32T1: read 588, write 499
    - RND4K Q1T1: read 82, write 181

Conclusions:

- switching from Windows 10 to 11 increases the GPU performance by 3%
- enabling fTPM decreases CPU perf by 0.8% and GPU perf by 0.5%
- enabling core isolation (memory integrity) decreases CPU perf by 0.4% and GPU perf by 0.8%
- installing AMD chipset drivers does not affect any performance
- updating BIOS from F17c (AGESA 1.2.0.8) to F19 (AGESA 1.2.0.B) did not affect any performance

## Worker

- Asus Dual Radeon RX 6650 XT OC V2 8GB
- Asus ROG Strix Z370-I Gaming
- Corsair SF600 Platinum
- Fractal Design Define Nano S
- Glorious PC Gaming Race Model O Wireless
- HyperX Fury Black 16GB 2133MHz DDR4 CL14
- Idobao ID80V3
- Intel Core i9-9900K
- LG 27UD88-W
- LG 27UL850-W
- Logitech C922 Pro Stream Webcam
- Noctua NH-U9S
- Samsung 840 Evo 500GB
- Samsung 970 Evo 1TB
- Vortex Race 3
- Zowie EC1-A

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

Settings saved with BIOS version 3005 to `worker-bios.cmo` and `worker-bios.txt` files.

Enabled SGX using [sgx-software-enable](https://github.com/intel/sgx-software-enable).

*Security Device Support* is required for Windows 11 but when it is enabled, `dmesg` reports `tpm_crb MSFT0101:00: [Firmware Bug]: ACPI region does not cover the entire command/response buffer.`

*Above 4G Decoding* causes a black screen when trying to run Reflect or Windows setup.

### Monitor settings

LG LG 27UL850-W was calibrated for Custom profile, brightness set to 30 and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 49 50. Software calibration is stored in the ICC profile `27ul850-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ul850-w.50.icm`. 

LG LG 27UD88-W was calibrated for Custom profile, brightness set to 30 and gamma mode 1 selected. Hardware calibration resulted in the RGB settings of 49 47 50. Software calibration is stored in the ICC profile `27ud88-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ud88-w.50.icm`. 

### Keyboard settings

For Idobao ID80V3 and ID80V2 follow [this guide](https://idobao.github.io/manuals/flashing/) to update the firmware and use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `id80v3-0.png` and `id80v3-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt+Fn+F10`.

## Drifter

- Dell XPS 13 9310
  - Intel Core i71165G7
  - LPDDR4x 32GB 4266MHz 
  - M.2 NVMe PCIe 1TB
- Samsung 850 Pro 256GB

## Others

- Kobo Clara BW
- KOSS Porta Pro
- OnePlus 9 8GB 128GB
- SanDisk Ultra Dual Drive 64GB
- SanDisk Ultra Dual Drive Luxe 128GB
- SanDisk Ultra Dual Drive Luxe 64GB
- Sennheiser HD 58X Jubilee
- Steam Deck OLED 1TB

