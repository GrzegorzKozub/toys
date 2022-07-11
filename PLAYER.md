# Player

## BIOS settings

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

## GPU undervolting

In Afterburner, set core clock to -290MHz and drag 875mV (or 900mV to prevent Metro Exodus crashes) point to 1920MHz.

