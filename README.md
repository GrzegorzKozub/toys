# Toys

Hardware list and settings

## Player

- AMD Ryzen 9 5900X
- Asus TUF RTX 3080 10GB Gaming
- Cooler Master NR200P
- Corsair SF750 Platinum
- G.SKILL Trident Z Neo 32GB 3600MHz DDR4 CL16
- Gigabyte B550I Aorus Pro AX
- Glorious PC Gaming Race Model O Wireless
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

For KBDfans D84 v2, use [VIA](https://www.caniusevia.com/) to program the keyboard. Current settings are in `d84v2.json`.

## Worker

- Asus ROG Strix Z370-I Gaming
- Corsair SF600 Platinum
- Fractal Design Define Nano S
- HyperX Fury Black 16GB 2133MHz DDR4 CL14
- Idobao ID80V2
- Intel Core i9-9900K
- LG 27UD88-W
- LG 27UL850-W
- MSI GeForce RTX 2080 Ti Gaming X Trio 11GB
- Noctua NH-U9S
- Samsung 840 Evo 500GB
- Samsung 970 Evo 1TB
- Vortex Race 3
- Zowie EC1-A
- Zowie EC2-A

### Firmware settings

Load from `worker-firmware-settings` file.

### Monitor calibration

LG LG 27UL850-W was calibrated for Custom profile, brightness set to 30 and gamma mode 2 selected. Hardware calibration resulted in the RGB settings of 50 49 50. Software calibration is stored in the ICC profile `27ul850-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ul850-w.50.icm`. 

LG LG 27UD88-W was calibrated for Custom profile, brightness set to 30 and gamma mode 1 selected. Hardware calibration resulted in the RGB settings of 49 47 50. Software calibration is stored in the ICC profile `27ud88-w.30.icm`. Brightness set to 50 resulted in the same hardware settings and `27ud88-w.50.icm`. 

### Keyboard settings

For Idobao ID80V2, use [QMK Configurator](https://config.qmk.fm/#/) to customize the keymap and compile the firmware. Then use [QMK Toolbox](https://github.com/qmk/qmk_toolbox/releases) to flash the firmware to the keyboard. Hold `Fn` + `Esc` to connect the keyboard in DFU mode. Current keymap is in `id80v2.json` and current firmware is in `id80v2.hex`.

## Drifter

- Dell XPS 13 9310
- Samsung 850 Pro 256GB
- SanDisk Ultra Dual Drive Luxe 128GB
- SanDisk Ultra Dual Drive Luxe 64GB

