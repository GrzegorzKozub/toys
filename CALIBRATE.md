# MSI MPG 321URX calibration

Tools: Calibrite Display Plus HL, [DisplayCAL](https://displaycal.net/) ([ArgyllCMS Windows download](https://www.argyllcms.com/downloadwin.html)), HCFR, Windows 11.

Room: bright, white walls, window with sunlight hitting the room (ambient only, not direct glare on the panel). Preference for lower overall brightness; occasional dim-evening gaming.

## Known risk

The Display Plus HL's sensor is tuned for high-luminance panels and reads close to 0 nits in deep shadows on OLED/QD-OLED, causing elevated/wonky gamma in near-black. This is a hardware limitation, not fixable with a correction matrix. Accepted tradeoff: white point, mid-tone gamma, and overall brightness will be solid; shadow-detail accuracy is the known weak point.

Calibrite's own "OLED" preset is tuned for RGB AMOLED (phones), not QD-OLED. No QD-OLED-specific CCSS exists for this device; the closest community substitute is "RGB OLED" CCSS, an approximation.

## 0. Prep

- Update monitor firmware and GPU drivers.
- NVIDIA Control Panel: confirm output dynamic range is **Full** (not Limited) and desktop color depth/format matches the panel. This sits below any ICC profile and can silently clip regardless of calibration quality.
- Let the panel run 20-30 min before measuring.
- Darken the room for the actual colorimeter measurement session, even though the monitor is used in a bright room day to day.

## 1. OSD setup

- Pro Mode: **User (vivid, native wide gamut)** - the maintained default. sRGB (clamped) is documented below as an alternative path for the record, not a second maintained profile; there is no OS-level hook to auto-swap ICC profiles on OSD Pro Mode changes (unlike the SDR/HDR toggle), so running two SDR Pro Modes day-to-day would be fully manual with no tooling to sync them. Not pursuing that.
- Turn Windows HDR off entirely for this pass (not just relying on OSD DisplayHDR state).
- MSI OLED Care: Static Screen Detection OFF, Multi Logo Detection OFF.
- Remove/unassign any previously installed ICC profile (e.g. TFT Central's `mpg321urx.34.icm`) in Settings > System > Display > Color profile (or classic Color Management > Devices), and reset the video card gamma table (VCGT) to linear via DisplayCAL (Options > reset video card gamma table). A leftover VCGT reshapes the signal before the colorimeter reads it, corrupting the native characterization.
- Reset OSD brightness, contrast, and color temp/RGB to factory defaults - one clean, known starting point before any measurement.

## 2. Locking brightness, contrast, and RGB gain before profiling

1. From factory defaults, run **DisplayCAL's black-level tool** to sweep contrast values and find where near-black/near-white clipping starts on this unit. Lock OSD contrast to that value (don't reuse TFT Central's contrast of 70 - it's for their sample, not this one). The factory default Contrast value isn't documented in the manual or any review - check it directly on the unit via OSD "Reset -> Yes" before adjusting, if you want to know the true out-of-box starting point.
2. With contrast now locked, dial in **OSD brightness toward 120 nits** using the hardware slider (not a software/LUT-only reduction, which burns bit depth and can band). Take live colorimeter readings while adjusting - don't trust extrapolated OSD-value-to-nits guesses without confirming, since the curve isn't guaranteed linear across the range and shifts if contrast changes.
   - Measured reference point on this unit (contrast not yet finalized when taken, re-verify after step 1): brightness 34 -> ~120 nits. Already a direct match for the 120 nit target - confirm/fine-tune from there rather than starting blind.
3. **Hardware white balance via DisplayCAL's Interactive display adjustment**: enable the "Interactive display adjustment" checkbox on the Calibration tab before starting a calibration run. DisplayCAL shows a live RGB/luminance error readout against a full-screen patch while you manually adjust OSD Color Temperature > User/Custom RGB gain sliders - tweak R, G, B one at a time until the displayed error nulls out as close to zero as the sliders allow. This derives this unit's own RGB gain values, the same way TFT Central's published `RGB 97 99 100` was derived for their sample (not reusable across units - panel variance). Record the resulting values here once found: **SDR calibrated at OSD Brightness 34 (~120 nits), RGB 96 99 99** (this unit, Pro Mode vivid, contrast locked per step 1).
4. Re-measure contrast, brightness, and white balance after any of the three changes, since they interact on OLED (contrast affects the peak luminance ceiling, and RGB gain changes shift both).

## 3. SDR calibration (DisplayCAL)

- Fresh calibration, native wide gamut (User/vivid). Do not reference TFT Central's `mpg321urx.34.icm` as a shortcut - panel variance between units makes it unreliable even though the Pro Mode now matches; measure independently.

### Quick reference

| Tab | Setting | Value |
|---|---|---|
| Display & instrument | Display | MSI MPG 321URX |
| Display & instrument | Instrument | Calibrite Display Plus HL |
| Display & instrument | Correction | "RGB OLED" CCSS |
| Display & instrument | White level drift compensation | checked |
| Display & instrument | Black level drift compensation | off |
| Calibration | Interactive display adjustment | checked |
| Calibration | White point | Color temperature, 6500K, Neutral |
| Calibration | Tone curve | Gamma 2.2 |
| Calibration | White level | 120 cd/m2 (not "As measured") |
| Calibration | Black level | As measured |
| Calibration | Black output offset | 0% |
| Calibration | Black point correction | 0% |
| Profiling | Profile type | Curves + matrix |
| Profiling | Testchart | Default/Auto |
| Profiling | Profiling quality | Medium |
| Profiling | Black point compensation | off |

### Display & instrument tab

- Display: the MSI MPG 321URX.
- Instrument: Calibrite Display Plus HL (i1DisplayPro-family).
- Correction: "..." next to the correction dropdown > Install extra files... > the "RGB OLED" CCSS downloaded from https://colorimetercorrections.displaycal.net/ > select it once installed.
- White level drift compensation: **checked** - periodically re-measures a reference white patch and normalizes readings against it, correcting for OLED ABL/thermal brightness drift during the measurement run itself. Driven by display behavior, not profile size, so it applies even to a short Curves + matrix run - negligible time cost, no accuracy downside. Missed in earlier runs; add going forward.
- Black level drift compensation: **off** - compensates for instrument sensor drift, not display drift; mainly needed for non-temperature-stabilized spectrometers (i1Pro, ColorMunki). The Display Plus HL is a temperature-stable colorimeter, so this isn't needed.

### Calibration tab

- Interactive display adjustment: **checked** - guided live-feedback pass to set OSD brightness and RGB gain by hand (see section 2.3).
- White point: dropdown to **Color temperature**, enter **6500** (K), "Neutral" checked - not "As measured" (which would just accept the panel's native, possibly off-D65, tint uncorrected).
- Tone curve: **Gamma 2.2** (plain power curve, not "sRGB").
- White level: uncheck "As measured", enter **120** cd/m2 directly.
- Black level: leave as "As measured" - native OLED black, not a raised/clamped target.
- Black output offset: **0%** - preserves native OLED black; 100% would raise the black floor and lose contrast.
- Black point correction: **0%** (or as low as possible) - skipped because it depends on near-black chromaticity, which is exactly where the Display Plus HL's sensor is least reliable on this panel; not worth chasing sensor noise into a spurious hue correction.

### Profiling tab

- Profile type: **Curves + matrix** (this is "matrix-only" in DisplayCAL's terminology) - not "LUT", to avoid the known QD-OLED undersaturation / near-black-elevation artifact from full 3D LUT profiles.
- Testchart: DisplayCAL auto-selects an appropriate smaller chart for curves+matrix; no manual patch selection needed.
- Profiling quality: **Medium** is sufficient for a matrix profile (higher settings mainly benefit LUT profiles with far more patches).
- Black point compensation: **off** - mostly affects perceptual/saturation intent tables that a curves+matrix profile and typical desktop/gaming rendering don't use, and it shares the same near-black-measurement-reliability problem as black point correction above.

### After profiling

- Verify the resulting profile with HCFR.
- Caveat: in native wide-gamut mode, only color-managed apps (browsers, Adobe) will render through this ICC profile correctly. Most non-color-managed content (many games, general desktop UI) sends sRGB-range values straight to the panel and will look oversaturated regardless of calibration quality, since that content never consults the profile. Windows Auto Color Management (24H2+) is not a fix for this - it's recommended off when running a custom DisplayCAL profile, since it conflicts.

### Verification results

Run 1 (Black output offset wrongly at 100%, RGB white balance step skipped):

- Brightness error = 0.035431 cd/m2 (is 120.035431, should be 120.000000)
- White point error = 0.669841 deltaE
- Maximum neutral error (@ 0.194481) = 1.548315 deltaE
- Average neutral error = 0.580945 deltaE

Run 2 (redone: Black output offset corrected to 0%, RGB white balance done via Interactive display adjustment - SDR calibrated at OSD Brightness 34 / ~120 nits, RGB 96 99 99):

- Brightness error = -0.940217 cd/m2 (is 119.059783, should be 120.000000)
- White point error = 0.200057 deltaE
- Maximum neutral error (@ 0.005582) = 15.256335 deltaE
- Average neutral error = 0.675277 deltaE
- Log noted "Black point device hack is enabled" (DisplayCAL/Argyll's built-in workaround for the i1d3/HL near-black weakness).

Run 2 improved white point error substantially (0.67 -> 0.20 dE) from actually doing the RGB white balance step. The large max neutral error jump (1.55 -> 15.26 dE) is not a regression - it's measured at ~0.56% of white (~0.67 nits, essentially true black), which run 1 never actually reached because its wrong 100% black output offset artificially raised the floor and masked the Display Plus HL's known near-black sensor weakness (see "Known risk" above). A large color error at that luminance level is below normal visual perception and consistent with the accepted hardware limitation, not a new problem.

Run 3 (White level drift compensation enabled, per section 3 fix above):

- Brightness error = -1.255209 cd/m2 (is 118.744791, should be 120.000000)
- White point error = 0.278570 deltaE
- Maximum neutral error (@ 0.194481) = 1.151258 deltaE
- Average neutral error = 0.521954 deltaE
- White drift was 0.636150 deltaE - confirms White level drift compensation is actively measuring and correcting for real OLED ABL/thermal drift during the run, not a no-op.
- Instrument reported "No distinct refresh period" during setup (a pre-run diagnostic on a separate attempt logged an odd 38.9 Hz refresh reading - given this run's clean result, that number looks like measurement noise rather than a real timing problem, and isn't worth chasing further).

**Run 3 is the best result so far**: best average neutral error (0.52 dE) and best max neutral error (1.15 dE, back at the mid-tone patch rather than run 2's near-black sensor-noise spike) of the three runs. White point error (0.28 dE) is close behind run 2's best-of-three 0.20 dE. Brightness error (-1.26 cd/m2, ~1% off 120 nit target) is the worst of the three but still imperceptible in practice. This is the run to keep.

### Ambient light level adjustment (viewing conditions)

Separate from black point/black output offset. DisplayCAL's *Ambient light level* field (Calibration tab, `dispcal -A <lux>`) reshapes the tone curve to compensate for the actual room brightness the display is viewed in - distinct from and independent of the *Whitepoint* chromaticity setting, which stays fixed at *Color temperature 6500K, Neutral* regardless. Do not point the whitepoint target at the ambient reading's color: room lighting here is a variable, non-neutral mix of daylight and indoor light, and chasing it away from D65 would undermine every color-managed app's assumption of a standard reference white for no real benefit.

Measure ambient under the room's actual normal-use lighting, not the darkened room used for the calibration patches themselves - the two are unrelated requirements. Either use DisplayCAL's "Measure" button next to the field (instrument flipped to its ambient/diffuser mode, held beside the screen facing outward, not at the panel) before darkening the room, or the `dispcal` console's "6) Measure and set ambient for viewing condition adjustment" menu option, then proceed to darken the room and take the actual patch measurements as normal. A reading taken while the room was still dark/dim is not valid - one such stray reading came in at 21 lux, far below two independent readings of this room's real daytime ambient (100 and 105 lux, a week+ apart), and was discarded rather than used.

Comparison on the LG 27GP950-B (hardware settings unchanged between runs): a run with `-a105.0` vs. an otherwise-identical run with the flag omitted produced substantially different curves (e.g. input 0.40 mapped to output ~0.33 with the ambient adjustment vs. ~0.40 without - the non-adjusted run reads visibly brighter in mid/shadow tones). The ambient-adjusted run also verified with better average/max neutral (grayscale) error and a higher realized contrast ratio, at the cost of marginally larger (still sub-perceptible) brightness and white point error. Caveat: the non-adjusted comparison run had a leftover ICC profile applied during measurement rather than a reset/linear VCGT, so the size of that gap isn't fully clean - but the curve-reshaping mechanism itself is confirmed directly from the `dispcal` command line and calibration curve data regardless of that confound.

Always confirm the ICC profile is unassigned and the video card gamma table is reset to linear (see Prep) before *every* calibration run, including retries - not just the first-ever setup pass on a given display.

Three runs on the LG 27GP950-B, hardware settings unchanged throughout:

| Metric | `-a105` | no `-a` | `-a500` |
|---|---|---|---|
| Viewing conditions power | 1.222 | - | 0.968 |
| Brightness error | 0.59 cd/m2 | 0.30 cd/m2 | 0.38 cd/m2 |
| White point error | 0.18 dE | 0.16 dE | 0.55 dE |
| Max neutral error | 1.24 dE | 1.44 dE | 1.50 dE |
| Average neutral error | 0.58 dE | 0.75 dE | 0.82 dE |
| White drift (verify) | 0.98 DE | 0.49 DE | 0.63 DE |
| Realized contrast ratio (report) | 836:1 | 594:1 | 516:1 |
| Measured black level (report) | 0.1444 cd/m2 | 0.2026 cd/m2 | 0.2332 cd/m2 |

`-a105` remains the best result: best contrast ratio by a wide margin and best average/max neutral error, at the cost of a still-imperceptible brightness/white point error. The `-a500` run is the worst result of the three on nearly every metric (white point error alone is 3x worse than either other run), because 500 lux doesn't correspond to any real measurement of this room - it was a typed/leftover field value, not a fresh reading, and roughly 5x the two independently confirmed readings of this room's actual daytime ambient (100 and 105 lux). Lesson: a wrong ambient value is worse than no ambient adjustment at all - always confirm the lux figure being fed to `-a` against a real, fresh measurement of the room before starting the run, don't trust a value already sitting in the field.

### Alternative path: sRGB (clamped) - for the record, not actively maintained

- Same calibration procedure (D65, gamma 2.2, 120 nits, matrix-only, RGB OLED CCSS, DisplayCAL black-level contrast test), just with OSD Pro Mode set to sRGB instead of User.
- Benefit over vivid: the monitor clamps to sRGB gamut in hardware, so non-color-managed content displays correctly without relying on per-app color management.
- Not pursued as a parallel profile because switching OSD Pro Mode has no matching automatic ICC profile switch in Windows - would require fully manual switching of both together, or a custom hotkey script (e.g. ControlMyMonitor + a profile-switch command) bound together. Revisit this path if the oversaturation on non-color-managed content in vivid mode turns out to be more annoying in practice than expected.

## 4. HDR setup

- Pro Mode (User/vivid vs sRGB clamped) is an SDR-only setting. HDR bypasses it: the panel decodes HDR through its own EOTF/tone-mapping pipeline and renders in native wide gamut regardless of the SDR Pro Mode selection (strong inference from a sibling MSI QD-OLED model's manual explicitly graying out its sRGB/Adobe RGB clamp under HDR; not independently confirmed for this exact SKU). So the HDR profile plan is unaffected by the vivid-vs-clamped decision above - it was always targeting native gamut.
- Keep OSD DisplayHDR set to Peak 1000 nits.
- Re-enable Windows HDR; run the Windows 11 HDR Calibration app for slider-based min/max luminance tuning.
- Assign both an SDR profile and an HDR profile to the display in Windows Color Management (Devices tab > "Use my settings for this device"). Rely on Windows' native auto-switching between them.
- Known issue: switching can glitch after sleep or exiting a fullscreen HDR game/app, leaving the wrong profile active. Fix with `Ctrl+Win+Shift+B` (graphics reset) or manually reselect the profile in Color Management.

## 5. Day/evening use

- One calibrated SDR profile (120 nits), no separate evening ICC pass.
- Nudge the OSD brightness slider down manually for dim evening sessions rather than recalibrating.

## 6. Maintenance

- Recalibrate every 3-6 months, or after firmware updates.

## Sources / notes

- TFT Central best-settings guide page (https://tftcentral.co.uk/best-settings/best-settings-guide-for-the-msi-mpg-321urx) has no public brightness/nits table; specific figures live in their linked video or Patreon-gated ICC database, so any brightness-to-nits mapping should be independently measured rather than trusted from secondhand notes.
- Windows Advanced Color ICC profile switching (SDR/HDR) is a real, documented OS feature, but unreliable in practice per Microsoft support threads and community reports (PC Monitors, DisplayCAL, ElevenForum forums).
- MSI MPG 321URX long-term burn-in test (OLED-info, 21 months / 5000+ hours): only 2% brightness degradation - automatic OLED Care mitigations handle most longevity concerns regardless of chosen brightness.
