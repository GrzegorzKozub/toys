# Toys

Hardware manifest and settings

## Player

- AMD Ryzen 9 5900X
- Asus TUF RTX 3080 10GB Gaming
- Cooler Master NR200P
- Corsair SF750 Platinum
- G.SKILL Trident Z Neo 32GB 3600MHz DDR4 CL16
- Gigabyte B550I Aorus Pro AX
- Glorious PC Gaming Race Model O 2 Wireless
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
  - IO Ports
    - Above 4G Decoding: Enabled
    - Re-Size BAR Support: Auto
  - Platform Power
    - Wake on LAN: Disabled
  - Miscellaneous
    - LEDs in System Power On State: Off
    - AMD CPU fTPM: Enabled
  - AMD CBS
    - CPU Common Options
      - Global C-state Control: Enabled
- Smart Fan 5
  - CPU_FAN
    - Curve: 0c 0%, 40c 30%, 55c 40%, 60c 50%, 70c 80%, 80c 100%
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

### Monitor calibration

LG 27GP950-B was calibrated for Gamer 1 profile, brightness set to 10 and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 50 47. Software calibration is stored in the ICC profile `27gp950-b.b10.icm`.

### Keyboard settings

For KBDfans D84 v2, use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `d84v2-0.png` and `d84v2-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt`+`PrintScreen`.

### Benchmarks

- Windows 10 21H2 19044, NVIDIA 526.86
  - Cinebench R23.200 (pts)
    - fTPM off: multi core 21489, single core 1393
    - fTPM on: multi core 21305, single core 1388
  - 3DMark 5.54.1138 Time Spy 1.2 (pts)
    - fTPM off: 16438, graphics score 17224, CPU score 13061
    - fTPM on: 16354, graphics score 17063, CPU score 13239
  - Superposition 1.1.8628 4K (pts)
    - fTPM off: 14291
    - fTPM on: 14277
  - CrystalDiskMark 8.0.4 (MB/s)
    - NVME 1:
      - SEQ1M Q8T1: read 6871.93, write 4960.95
      - SEQ1M Q1T1: read 4232.27, write 4346.58
      - RND4K Q32T1: read 565.02, write 410.70
      - RND4K Q1T1: read 86.36, write 210.73
    - NVME 2:
      - SEQ1M Q8T1: read 3235.74, write 3174.55
      - SEQ1M Q1T1: read 2712.19, write 2887.07
      - RND4K Q32T1: read 584.33, write 473.30
      - RND4K Q1T1: read 83.55, write 186.64
- Windows 11 22H2 22621, NVIDIA 526.86
  - Cinebench R23.200 (pts)
    - fTPM on: multi core 21501, single core 1380
  - 3DMark 5.54.1138 Time Spy 1.2 (pts)
    - fTPM on: 16921, graphics score 17761, CPU score 13345
  - Superposition 1.1.8628 4K
    - fTPM on: 14625  
  - CrystalDiskMark 8.0.4 (MB/s)
    - NVME 1:
      - SEQ1M Q8T1: read 6864.65, write 5011.48
      - SEQ1M Q1T1: read 4177.83, write 4325.48
      - RND4K Q32T1: read 545.13, write 400.57
      - RND4K Q1T1: read 84.04, write 193.85
    - NVME 2:
      - SEQ1M Q8T1: read 3238.47, write 3166.61
      - SEQ1M Q1T1: read 2708.94, write 2886.74
      - RND4K Q32T1: read 561.62, write 452.48
      - RND4K Q1T1: read 81.13, write 173.23

## Worker

- Asus ROG Strix Z370-I Gaming
- Corsair SF600 Platinum
- Fractal Design Define Nano S
- Glorious PC Gaming Race Model O Wireless
- HyperX Fury Black 16GB 2133MHz DDR4 CL14
- Idobao ID80V3
- Intel Core i9-9900K
- LG 27UD88-W
- LG 27UL850-W
- MSI GeForce RTX 2080 Ti Gaming X Trio 11GB
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
- Intel Virtualization Technology: Enabled
- SATA6G_1(Charcoal Black): Disabled
- SATA6G_3(Charcoal Black): Disabled
- SATA6G_4(Charcoal Black): Disabled
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

### Monitor calibration

LG LG 27UL850-W was calibrated for Custom profile, brightness set to 30 and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 49 50. Software calibration is stored in the ICC profile `27ul850-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ul850-w.50.icm`. 

LG LG 27UD88-W was calibrated for Custom profile, brightness set to 30 and gamma mode 1 selected. Hardware calibration resulted in the RGB settings of 49 47 50. Software calibration is stored in the ICC profile `27ud88-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ud88-w.50.icm`. 

### Keyboard settings

For Idobao ID80V3 and ID80V2 follow [this guide](https://idobao.github.io/manuals/flashing/) to update the firmware and use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `id80v3-0.png` and `id80v3-1.png`.

To access the [SysRq](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_(SysRq)) key use `Alt`+`Fn`+`F10`.

## Drifter

- Dell XPS 13 9310
  - Intel Core i71165G7
  - LPDDR4x 32GB 4266MHz 
  - M.2 NVMe PCIe 1TB
- Samsung 850 Pro 256GB

## Others

- KOSS Porta Pro
- OnePlus 9 8GB 128GB
- SanDisk Ultra Dual Drive 64GB
- SanDisk Ultra Dual Drive Luxe 128GB
- SanDisk Ultra Dual Drive Luxe 64GB
- Sennheiser HD 58X Jubilee
- Steam Deck 512GB

