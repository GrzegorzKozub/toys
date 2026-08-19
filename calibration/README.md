# Canonical display calibration

An alternative, from-scratch calibration approach based on [DisplayCAL's own documentation](https://displaycal.net/#concept) and canonical forum guidance, rather than accumulated trial and error. No specific brightness/goal target - defaults are trusted unless there's a documented, specific reason to deviate.

Tools: Calibrite Display Plus HL, [DisplayCAL](https://displaycal.net/) ([ArgyllCMS Windows download](https://www.argyllcms.com/downloadwin.html)).

## Concept

Two separate steps, both required:

- **Calibration**: measures the display and writes correction curves into the GPU's video card gamma table (VCGT) to hit target brightness/gamma/white point. Affects everything on screen, color-managed or not.
- **Profiling**: measures the *calibrated* display's actual behavior and writes an ICC profile describing it (with the VCGT curve embedded inside, via the `vcgt` tag). Only benefits color-managed applications - non-color-managed apps only see the calibration step's effect.

A calibrated-but-unprofiled display, or use in a non-ICC-aware app, will still look somewhat off - both steps matter.

## Prep

- Update monitor firmware and GPU drivers.
- Reset the monitor settings and disable any post-processing or OLED care features.
- Unassign any color profile and disable HDR in Windows.
- Let the display warm up for at least 30 minutes before calibrating.
- On Intel laptops, ensure *Enhanced Power Saving* (DPST), *Panel Self Refresh* and *Lighting Aware Contrast Enhancement* are disabled in Intel Graphics Software - all three are content/ambient-reactive power features that can corrupt a measurement pass with drift. Independent of and not controlled by Windows' own "adaptive brightness" toggle.

## Settings

1. In DisplayCAL, enable Options -> Show advanced options.
2. Setup *Display & instrument* tab:
  - Select *i1 DisplayPro* instrument.
  - Set *Mode* to *Refresh (generic)* on OLED, *LCD (generic)* on LCD.
  - Enable *White level drift compensation* (real, verified OLED ABL/thermal drift mitigation - not ambient light related, safe/cheap to leave on for any panel).
  - Leave *Black level drift compensation* off - compensates for spectrophotometer thermal drift (i1 Pro, ColorMunki), not needed for this colorimeter.
  - Correction: use a spectral CCSS file (prefer finer resolution, i1 Pro reference) from the [Colorimeter Corrections Database](https://colorimetercorrections.displaycal.net/) for the MSI and LG monitors. **For the laptop: no correction file** - only a community CCMX exists for this model, and canonical guidance explicitly advises against using CCMX for a general calibration ("CCMX you should not try it"). Use the instrument's uncorrected default response instead.
3. Setup *Calibration* tab:
  - Leave *Interactive display adjustment* enabled - guided live-feedback pass to set OSD brightness/RGB by hand where the monitor has hardware controls (skip for the laptop, which has none).
  - Set *Whitepoint* to *Color temperature* of *6500 K* (D65) - the canonical default target.
  - Set *Tone curve* to *Gamma 2.2* - canonical default, matches most displays' native response shape.
  - Set *White level* to **As measured** - no specific brightness target. Find an eye-pleasing OSD brightness level first (by feel, not a chased cd/m2 number), then calibrate to whatever that native level actually measures as.
  - Leave *Black level* at *As measured* - native black level, not a raised/clamped target, maximizes contrast ratio.
  - Leave *Black output offset* at the preset default, **100%**. Initially assumed (wrongly) that 0% preserves native black better - it's the opposite. Per Argyll's own `dispcal` documentation: the 0-100% scale controls whether the unavoidable native-black/ideal-curve mismatch is absorbed on the curve's *input* side (0%) or *output* side (100%). 100% scales the whole target curve to match the display's actual native black, preserving contrast ratio - which is exactly why Argyll changed its own default to `-f 1.0` ("to make it work in more sympathy to a typical display response"). 0% pushes the correction onto the input side instead, which lifts near-black shadow detail and measurably costs contrast - confirmed empirically on the LG 27GP950-B, where a 100% run produced the best contrast ratio of every test run on that display (925:1) and a 0%-equivalent run one of the worst. No override needed here - the preset default is already correct.
  - *Black point correction*: leave at the preset default (**0%**) on the MSI OLED - near-black chromaticity is unreliable on this instrument/panel combo, so a hue correction there risks introducing a spurious shift rather than fixing a real one, and OLED has no backlight to leak color in the first place. On the three LCDs (LG 27GP950-B, LG 27UL850-W, laptop), set to **Auto** rather than a fixed override - it measures each panel's actual black level/hue deviation and computes a tailored correction instead of assuming a flat percentage. Empirically the best-performing choice tested on the LG so far (computed ~39%, produced the second-best contrast ratio and best-or-tied-best neutral tracking of every run on that display) - a flat 100% override is defensible but unconfirmed as better.
  - **Do not use *Ambient light level adjustment***. DisplayCAL's own developer describes this as "a gimmick" for color-managed work - it's undone by color management for color-managed apps and can itself cause calibration problems. The one legitimate use (simulating a fixed alternate viewing environment via CIECAM02 gamut mapping) is a Profiling-tab advanced feature, not this. Note for anyone cross-referencing old notes: the ambient-light flag is `dispcal -a <lux>` (lowercase); `-A <rate>` is an unrelated parameter for the Auto black point correction rate above - easy to confuse since they share a letter.
4. Setup *Profiling* tab:
  - Set *Profile type* to **Curves + matrix on the MSI OLED** (the one deviation from pure uniformity - full 3D LUT profiles have a documented QD-OLED-specific near-black undersaturation/elevation issue) and **XYZ LUT + matrix on the three LCDs** (canonical general default - most accurate, broadly compatible via its included matrix fallback).
  - Leave *Profile quality* at its default/Auto - "the defaults are chosen sensibly, so in 99.9% of cases you'll just get the best result possible right out of the box" (DisplayCAL developer).
  - *Black point compensation*: DisplayCAL auto-toggles this based on Profile type - selecting **Curves + matrix auto-enables** it, selecting **XYZ LUT + matrix auto-disables** it. Observed directly in the UI, not from prior analysis. This is the opposite pairing from what the old (pre-canonical) reasoning wanted (OLED/Curves+matrix off, LCD/LUT+matrix on) - if that preference still matters, it needs a manual override in both cases rather than relying on the automatic default either way. Otherwise, leave it at whatever each profile type auto-sets.
  - Leave *Test chart* at *Auto-optimized* - suitable for essentially all non-specialist cases; don't hand-pick patch counts.
5. Run *Calibrate & profile*.

## Fresh install: Settings preset and Correction per display

DisplayCAL's *Settings* dropdown (top of the main window) offers built-in presets that pre-fill the Calibration/Profiling tabs for common cases. Matched against the canonical targets settled on above (D65, gamma 2.2, no forced white level):

- MSI MPG 321URX
  - Settings preset: **Office & Web (D65, Gamma 2.2)**
  - Why: the only built-in preset whose name explicitly commits to both settled targets (D65 whitepoint + gamma 2.2) - *Default (Gamma 2.2)* doesn't name a whitepoint at all, likely leaving it native instead of D65.
- LG 27GP950-B
  - Settings preset: **Office & Web (D65, Gamma 2.2)**
  - Why: same reasoning - explicit D65 + 2.2 match.
- LG 27UL850-W
  - Settings preset: **Office & Web (D65, Gamma 2.2)**
  - Why: same reasoning.
- Dell XPS 13 9310 (laptop)
  - Settings preset: **Office & Web (D65, Gamma 2.2)**, not *Laptop (Gamma 2.2)*
  - Why: *Laptop* doesn't name a whitepoint either - unverified whether it defaults to D65 or native. Since the canonical target is explicitly D65 for all four displays, prefer the preset that states it, even on the laptop. Worth checking what fields *Laptop* actually populates before trusting it, if curious.

Not fully verified: I haven't confirmed exactly which fields each preset populates beyond what their names state (e.g. *Default*'s actual whitepoint handling) - if *Office & Web* turns out to set anything else undesirable, check its populated fields against the Settings section above before relying on it blindly.

For **Correction** on a fresh install with no prior downloaded files, current dropdown contents (per `corrections.png`) split into two groups:

- **Device-specific spectral files already known-good for these exact displays** (re-download from [Colorimeter Corrections Database](https://colorimetercorrections.displaycal.net/) if starting truly fresh):
  - MSI MPG 321URX -> `Spectral: Unknown (MSI 321URX (i1 Pro 2, CIE 1931 2°))`
  - LG 27GP950-B -> `Spectral: Unknown (LG ULTRAGEAR+ 27gm950b_full white (i1 Pro 2))`
  - LG 27UL850-W -> `Spectral: LCD White LED IPS (LG 27UL850W by 4KM (i1 Pro 2))`
- **Dell XPS 13 9310 (laptop) - no device-specific spectral file exists**, only a community `Matrix: XPS13 Korrektur` CCMX, already decided against per canonical guidance ("CCMX you should not try it" for a general calibration). That decision (**None**) still stands.

Best built-in (no download) fallback per display, matched by actual panel technology rather than picked arbitrarily - useful if the device-specific files above were ever lost/unavailable:

- MSI MPG 321URX (QD-OLED) -> `Spectral: RGB OLED family (Sony PVM-2541, Samsung Galaxy S7, Lenovo LEN4140)`. Imperfect - this is tuned for phone-style RGB AMOLED, not Samsung Display's QD-OLED - but it's the closest built-in category; no built-in QD-OLED entry exists. Consistent with Calibrite's own OLED preset also being AMOLED-tuned, not QD-OLED-specific.
- LG 27GP950-B (Nano IPS, wide gamut, ~98% DCI-P3) -> one of the `Spectral: LCD PFS Phosphor WLED IPS` wide-gamut entries, closest being `99% P3 (MacBook Pro Retina 2016)` or `98% Adobe RGB/96% P3 (HP DreamColor Z24x G2)`. Not fully verified against this exact model's precise gamut percentage - treat as a reasonable approximation. Plain `LCD White LED family` would be the wrong category here since it implies standard, not wide, gamut.
- LG 27UL850-W (standard-gamut White LED IPS) -> `Spectral: LCD White LED family (AC, LG, Samsung)`. Confirmed by DisplayCAL's own categorization of the existing device-specific file for this exact monitor under "LCD White LED IPS," not one of the wide-gamut PFS Phosphor entries.
- Dell XPS 13 9310 (Sharp SHP14FA, standard-gamut IPS) -> `Spectral: LCD White LED family (AC, LG, Samsung)`, same reasoning as the LG 27UL850-W.
- General rule for any other future display: match by actual backlight/panel technology (wide-gamut phosphor/quantum-dot vs standard White LED vs OLED subpixel structure), not alphabetically. When genuinely unsure of the underlying technology, **None** is still a defensible default over guessing wrong - a mismatched generic correction can be worse than no correction at all.

## Verification procedure

Two distinct tools, both under DisplayCAL's Tools/Report menu - learn and use these instead of re-deriving settings from storage folder archaeology after the fact:

- **Report on calibrated/uncalibrated display**: takes a quick live reading of the display's current state (Black level, White level, gamma, contrast ratio, white chromaticity). "Uncalibrated" measures the panel's raw native response (no VCGT applied); "calibrated" measures whatever is currently actually loaded/active. Useful as a fast health-check without running a full profiling pass.
- **Verify calibration**: measures the display and compares it against a specific ICC profile you select (defaults to the currently active one), reporting Brightness error, White point error, Maximum/Average neutral error, and White drift.

Neither tool depends on the Calibration or Profiling tab settings described above - those tabs are instructions for *creating* a new calibration/profile, and are inert for Report/Verify. What *does* matter for both: the correct Display and Instrument selected, and the correct Correction file loaded on the Display & instrument tab. Verify additionally needs the correct profile and testchart selected - it reads its target values directly from that profile file, not from any UI field.

If the profile was assigned via Windows Settings/Color Management (rather than through DisplayCAL's own install step) after DisplayCAL was already open: no need to restart DisplayCAL - Windows applies the profile's VCGT to the GPU immediately, no reboot/logoff/app-restart required. Do explicitly reselect/confirm the correct profile in Verify's "Profile to verify" field before running it, though - if DisplayCAL was already open from the calibration session, it may still hold a reference to whatever profile it last worked with in that session rather than automatically re-querying Windows for the current default association.

## Known issues (DisplayCAL / ArgyllCMS / Windows)

- **Ambient light adjustment** - discouraged for color-managed use (see above); not a workaround for a fluctuating room.
- **Windows GDI `SetDeviceGammaRamp` memory leak** (Windows 10 1903+) - DisplayCAL's Profile Loader restarts itself daily at 04:00 and after sleep/hibernation to mitigate.
- **Video LUT gets reset by fullscreen apps/games** - fullscreen D3D/OpenGL apps use a different gamma-ramp path than the desktop GDI call the Profile Loader uses; a Windows 10 Creators Update bug additionally resets the GPU LUT on entering fullscreen even when the app didn't request it.
- **Windows "Night Light" conflicts** - documented reports of calibration banding reappearing or color casts when Night Light/blue-light filter is active alongside a loaded calibration.
- **Windows 11 HDR profile-loading bug** - the Profile Loader can force-load the HDR ICC profile even while Windows is in SDR mode, overriding the correct SDR profile (tracked as an open issue on the actively-maintained fork, see below).
- **"Graphics drivers or hardware do not support loadable gamma ramps" error** - usually a broken/incomplete GPU driver install; at least one case traced to antivirus (Comodo) quarantining files Argyll/DisplayCAL needed.
- **Windows 11 24H2 (build 26100) instrument detection conflicts** - reports trace "Instrument Access Failed" errors to the Logitech LampArray Service (installed via Windows' Dynamic Lighting feature) - worth checking if any RGB peripheral's Dynamic Lighting integration is active and the colorimeter isn't detected.
- **Colorimeter correction accuracy caveat** - a correction built for one specific instrument+display pairing may not transfer well to a different unit of the "same" display due to manufacturing variation and generally low inter-instrument agreement on older colorimeters.
- **Project maintenance note** - the original displaycal.net project has had long gaps between updates; the actively-maintained fork with current bug tracking is [eoyilmaz/displaycal-py3](https://github.com/eoyilmaz/displaycal-py3) on GitHub. Upgrade to it (currently 3.9.19) - 3.9.18 fixed XYZ LUT profile corruption/an overly strict white point check (LUT-specific code path, affects the LG monitors' profile type) and, correcting an earlier mislabeling here, a **near-white** shadow/highlight crush in matrix/shaper profiles (issue #710/PR #724 - a Curves+matrix-specific rTRC/gTRC/bTRC endpoint bug collapsing near-white steps to pure white, not a near-black fix), and added a white-point-drift warning directly in measurement reports. Caveat: version-to-version accuracy hasn't been strictly monotonic in this project (one tracked issue shows a user getting measurably worse results on 3.9.17 than the original 3.8.9.3), and there's a maintainer-acknowledged, not-fully-resolved report of Black Point Compensation sometimes being force-enabled independent of the profiling-tab setting (issue #570, present 3.9.15-16, fixed 3.9.17 - not a live concern on 3.9.19, but worth knowing for any OLED profile made on an older point release) - re-verify a fresh baseline after any version change rather than assuming newer is strictly better.
- **OLED settings re-verified against this fork's tracker (2026-08-14)** - Curves + matrix profile type, and Black point correction 0% (not Auto) on the MSI OLED, both still hold. No QD-OLED-specific near-black issue found; the closest match (issue #635, "Black levels lifted on OLED," same i1Display3-family instrument) is root-caused to a Linux/KWin/Wayland interaction, not applicable here. No code change found to how Auto black point correction computes its value - it still requires a black-level read, so it still carries the same instrument sensor-noise risk near black that ruled it out for OLED in the first place.

## Reference: confirmed hardware facts (kept from prior work, not re-derived)

*MSI MPG 321URX*

- Factory Contrast: 70.
- Pro Mode options: User (vivid, native wide gamut) or sRGB (clamped).
- Navi Key Up/Down/Left/Right mapped to Brightness - easy to bump accidentally.
- MSI OLED Care: Static Screen Detection and Multi Logo Detection available, previously set OFF.
- DisplayCAL/EDID device name for correction lookup: `MPG321UX OLED`.

*LG 27GP950-B*

- Game Mode: Gamer 1 (factory).
- Gamma: Mode 2 (factory).
- Factory Contrast: 70.
- DisplayCAL/EDID device name: `LG ULTRAGEAR+` (shared across LG's entire Ultragear+ lineup - match by model in the correction's `DESCRIPTOR`, not just the device name).

*LG 27UL850-W*

- Profile: Custom.
- Gamma: Mode 2 (factory).
- Factory Contrast: 70.
- DisplayCAL/EDID device name: `LG HDR 4K` (also a shared generic name across LG's older 4K lineup).

*Dell XPS 13 9310 (Sharp SHP14FA panel)*

- DisplayCAL/EDID device name: `F40HY-LQ134R1`.
- No OSD/hardware RGB controls - Interactive display adjustment step doesn't apply.
- Intel Enhanced Power Saving (DPST), Lighting Aware Contrast Enhancement (LACE), and Panel Self Refresh (PSR) are real, independently-documented sources of measurement drift on Intel integrated graphics - disable all three before calibrating (see Prep).
- Toggling these off in the Intel Graphics Software UI is not sufficient on its own - checked the actual registry value (`FeatureTestControl` under `Class\{4d36e968-e325-11ce-bfc1-08002be10318}\0000`) and found the DPST force-disable bit (bit 5 / `0x10`) was **not** set (`0x1200`) even with the UI toggle off. Applied a registry fix (bit set, now `0x1210`) - confirm this bit directly rather than trusting the UI checkbox alone. No *Dynamic Backlight Control* equivalent exists in this laptop's BIOS/UEFI.
- The Windows Intel driver's own RGB adjustment (separate from OSD gain) is Windows/driver-side software, not persisted hardware - doesn't carry over to a Linux boot on the same machine, and has no bit-depth-preservation benefit over DisplayCAL's own VCGT. Not used.

## Results

*MSI MPG 321URX, 2026-08-10* (White level: As measured, native target ~118.7 cd/m2):

> Verified: brightness error 0.49 cd/m2, white point error 0.13 deltaE, average neutral error 0.51 deltaE, max neutral error 1.57 deltaE.

Compared against the old (pre-canonical) best result for this display, *2026-08-07* (fixed 120 cd/m2 target):

- White point error: 0.13 dE vs 0.22 dE - improved ~43%.
- Average neutral error: 0.51 dE vs 0.62 dE - improved ~17%.
- Max neutral error: 1.57 dE vs 1.55 dE - a wash.
- Brightness error: 0.49 vs 0.42 cd/m2 - not a fair direct comparison, since the old run chased a fixed target and this one has no target by design (As measured); both are imperceptible in practice regardless.
- White drift: 0.60 dE, in the same healthy range as the old run's ~0.6-0.64 dE.

The canonical, defaults-trusted approach matched or beat months of trial-and-error tuning on the metrics that matter most (white point, average neutral error), using a much simpler procedure.

*MSI MPG 321URX, 2026-08-13* (displaycal-py3 upgrade, otherwise same settings as the 2026-08-10 run):

> Verified: brightness error 0.19 cd/m2, white point error 0.21 deltaE, average neutral error 0.60 deltaE, max neutral error 1.37 deltaE, white drift 0.56 deltaE.

Mixed against 2026-08-10: better brightness error and max neutral error, worse white point and average neutral error, both runs comfortably in the same excellent tier (nothing above 0.6 dE except max neutral error). The version upgrade itself didn't change the underlying ArgyllCMS command line for this profile type (Curves + matrix bypasses `colprof`/BPC application entirely, confirmed directly from the session log) - differences here are run-to-run noise, not a software behavior change.

*MSI MPG 321URX, 2026-08-14*:

> Verified: brightness 106.27 cd/m2, contrast inf:1, white point error 0.16 deltaE, average neutral error 0.52 deltaE, max neutral error 1.39 deltaE, white drift 0.30 deltaE.

Best-balanced of the three most recent MSI runs (2026-08-10, 2026-08-13, this one) - never far behind whichever run holds the record on brightness error, white point, or max neutral error (all within ~0.02-0.2 dE), tied for best on average neutral error, and clearly best on white drift (0.30 dE, roughly half the next-best of 0.56-0.60 dE across the other two). This is the run to keep.

Note on White drift specifically: it's only ever reported for the MSI. White level drift compensation is enabled only on OLED (an ABL/thermal-drift mitigation) and intentionally disabled on all three LCDs, and Verify's drift metric is tied to that same setting - confirmed directly by checking all four displays' 2026-08-14 verification logs: only the MSI's contains a "White drift" line, the three LCD logs don't have one at all. Not a missing/broken metric on the LCDs, expected behavior given the setting.

*LG 27GP950-B, 2026-08-14* (Black output offset 100%, Black point correction Auto ~39%, Gamma 2.2, 6500K):

> Verified: brightness error 0.13 cd/m2, white point error 0.76 deltaE, average neutral error 0.76 deltaE, max neutral error 1.52 deltaE. Report: contrast ratio 925:1 (best of every run tested on this display).

Best contrast ratio result on this display, confirming the Black output offset correction above empirically rather than just theoretically. Not yet a fully clean result, though: the live interactive white-point gain adjustment stalled at 0.7 dE before continuing instead of being pushed down near 0.1-0.2 dE like the tightest historical runs on this display achieved - white point error here is plausibly still capped by that, not by the settings themselves. A separate run with Auto BPC alone (100% -> old default output offset) got a better max/average neutral error (1.00/0.45 dE) and second-best contrast (757:1) despite the same convergence issue, suggesting Auto BPC's benefit is real and somewhat independent of output offset. Worth one more repeat with the gain actually converged tight before treating either number as final. A fourth test loading DisplayCAL's generic "sRGB" preset unmodified (As measured whitepoint, sRGB curve, same Auto BPC/100% offset) produced the misleadingly best-ever white point number (0.056 dE - an artifact of verifying a self-referential native-whitepoint target, not real accuracy) alongside the worst-ever near-black spike (8.92 dE) - confirms the earlier decision not to adopt a generic preset wholesale.

*LG 27UL850-W, 2026-08-14* (Black output offset 100%, Black point correction Auto ~35%, Gamma 2.2, 6500K, White level: As measured, native target ~118 cd/m2):

> Verified: brightness error 0.66 cd/m2, white point error 0.20 deltaE, average neutral error 0.41 deltaE, max neutral error 1.16 deltaE. Report: contrast ratio 783:1.

Best white point and average neutral error recorded for this display, live gain convergence at 0.4 dE (better than the recent 27GP950-B stalls, still short of the tightest historical runs). No prior baseline exists to compare against numerically - the last calibration on this display (2026-07-29) only recorded hardware settings, not verify results, and used the pre-correction settings (0% output offset, fixed 100% BPC). This stands as the first real accuracy baseline for this display.

*Dell XPS 13 9310 (Sharp SHP14FA), 2026-08-10* (White level: As measured, native target ~121.1 cd/m2):

> Verified: brightness error 6.81 cd/m2, white point error 0.33 deltaE, average neutral error 2.49 deltaE, max neutral error 5.90 deltaE.

**Bad result - the worst of every laptop session tried so far** (seven sessions total across this file's predecessor and this one), on four of five metrics: brightness error, max neutral error, average neutral error, and white drift (6.34 dE) all hit new worsts. Only white point error is unremarkable-but-fine.

The canonical settings changes (no correction file, no OSD pre-correction - this laptop has none) never addressed the actual root cause identified for this display: Intel DPST/LACE/PSR corrupting measurement with content/ambient-reactive brightness changes, a Windows/Intel-driver issue unrelated to DisplayCAL settings. White drift getting worse rather than better suggests those three settings may not still be disabled - possibly re-enabled by a Windows/driver update or a laptop restart since they were last turned off. Do not use this result; re-confirm DPST/LACE/PSR are actually off before recalibrating again.

*Dell XPS 13 9310 (Sharp SHP14FA), 2026-08-14* (Black point correction Auto, Gamma 2.2, 6500K, White level: As measured, native target ~120.9 cd/m2):

> Verified: brightness 113.43 cd/m2, contrast 1303:1, white point error 0.14 deltaE, average neutral error 1.15 deltaE, max neutral error 2.64 deltaE.

**Best laptop result across every session tried so far**, and not by a small margin: best white point error (tied with 2026-08-09's 0.142 dE), best max neutral error (2.64 dE, beating the prior best of 3.14 dE by ~16%), and best average neutral error (1.15 dE, beating the prior best of 1.33 dE). The only prior session in the same tier on any of these was the 2026-08-08 result (white point 0.52, max 3.14, avg 1.33) - this run beats it clean across all three.

Switching Black point correction from a fixed percentage to **Auto** (measured per-panel value rather than an assumed flat rate) is the most likely driver - the same change produced the best max neutral error and average neutral error of the whole series on the LG 27GP950-B the same day (see above). Two independent panels both landing best-of-series under Auto BPC is corroborating evidence, not a coincidence - worth treating as the standing recommendation for LCD generally, not a one-off.

Brightness error (7.51 cd/m2 against the As-measured native target) remains elevated, in the same tier as every session since 2026-08-10 - unresolved, and White drift is not tracked for this display (compensation intentionally left off for LCD - see the OLED/LCD split above), so this result doesn't confirm or rule out the drift instability seen on some earlier sessions. Use this as the reference profile going forward, with that caveat in mind.
