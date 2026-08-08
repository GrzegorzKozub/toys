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

Run 4 (2026-07-27 21:56, storage folder `...F-M 3xCurve+MTX`):

- Brightness error = -11.155863 cd/m2 (is 108.844137, should be 120.000000)
- White point error = 0.528927 deltaE
- Maximum neutral error (@ 0.950471) = 3.826842 deltaE
- Average neutral error = 1.458987 deltaE
- White drift was 0.388721 deltaE

Worst result recorded so far, and a different failure signature than the earlier near-black sensor-noise issue: the max neutral error landed near *white* (0.95), not near black, and the pre-run "Current calibration response" check showed White level at only 108.60 cd/m2 against the 120 target (~9.3% low) before profiling even started. This points to the OSD hardware brightness not actually being dialed to the target during Interactive display adjustment that session, not a profiling-quality or sensor problem - profiling quality/patch count cannot fix a mis-set hardware target.

Run 5 (2026-08-07 22:38, storage folder `...F-S 3xCurve+MTX`, Profiling quality set to High instead of Medium):

- Brightness error = 0.424965 cd/m2 (is 120.424965, should be 120.000000)
- White point error = 0.222741 deltaE
- Maximum neutral error (@ 0.143258) = 1.549901 deltaE
- Average neutral error = 0.620931 deltaE
- White drift was 0.709270 deltaE
- Pre-run check: White level 120.05 cd/m2 (essentially exact) and white chromaticity DE to daylight locus = 0.0 (essentially perfect D65) - confirms the hardware brightness/white balance was correctly dialed in this time, unlike run 4.

**Run 5 is the best result overall**, beating even run 3 on every metric except brightness error being effectively tied (both imperceptible). However, this is not a clean A/B test of Profiling quality (Medium vs High): the hardware brightness target was also fixed between run 4 and run 5, so the improvement can't be fully attributed to quality alone - unlike the earlier isolated contrast sweep. The quality setting plausibly contributed to the tighter neutral-error tracking (more patches better constraining the curve/matrix fit), but a real isolated test would require re-running at Medium quality with this same correctly-dialed hardware state before crediting High quality specifically.

### Ambient light level adjustment (viewing conditions)

Separate from black point/black output offset. DisplayCAL's *Ambient light level* field (Calibration tab, `dispcal -a <lux>`, lowercase - not `-A`, an unrelated black-point blend-rate flag) reshapes the tone curve to compensate for the actual room brightness the display is viewed in - distinct from and independent of the *Whitepoint* chromaticity setting, which stays fixed at *Color temperature 6500K, Neutral* regardless. Do not point the whitepoint target at the ambient reading's color: room lighting here is a variable, non-neutral mix of daylight and indoor light, and chasing it away from D65 would undermine every color-managed app's assumption of a standard reference white for no real benefit.

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

### Known issues - treat this feature with suspicion, not just caution

A separate incident on the MSI 321URX: the UI-displayed lux value did not match what was actually passed to Argyll, off by roughly a factor of 5x, and produced a bad calibration. No independently documented bug matching that exact symptom was found, but several separate, real problems with this feature were - together they're reason enough to distrust it by default rather than treat the LG result above as settled proof it's safe:

- DisplayCAL's own developer (Florian Höch) has repeatedly advised leaving this disabled unless there's a specific need - multiple forum threads report unexpectedly steep gamma/contrast results even at modest lux values, described by the dev himself as "screwball."
- Colorimeter-measured ambient lux can be wildly wrong independent of any UI bug - one report found a colorimeter's own ambient reading roughly 36x off from a reference spectrometer. This alone could produce a bad calibration that looks like a UI/CLI mismatch without actually being one.
- A confirmed code defect exists in DisplayCAL's "Measure ambient" workflow (a crash bug on at least one fork/version) - a real, distinct issue in that code path, separate from any value-mismatch concern.

Practical guidance going forward:

- Don't use this by default. Only use it with a real, dedicated lux meter reading (not the instrument's own "measure ambient" mode, given the reference-spectrometer discrepancy above) and a specific reason viewing-condition adjustment matters.
- If used, always check DisplayCAL's log for the actual generated `dispcal` command line and confirm the `-a` value matches intent before running the full session - do not trust the UI field alone, given the MSI incident above.
- The LG comparison table above still stands as a real, measured result for that specific session, but should not be read as proof the feature is generally safe - it's one data point from one session, sitting alongside a documented bad outcome from another.

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

## 7. Dell XPS 13 9310 laptop display (Sharp SHP14FA panel, DisplayCAL/EDID device name `F40HY-LQ134R1`)

Two calibration sessions compared, both 80 cd/m2 target, D6500 neutral, gamma 2.2, XYZ LUT + matrix profile type:

Session 1 (2026-08-02 20:55, storage folder `...F-S XYZLUT+MTX`):

- Report (pre-run): Black level 0.0897 cd/m2, White level 75.50 cd/m2 (target 80, ~5.6% low), gamma 2.16, contrast ratio 842:1, white chromaticity DE to daylight locus 1.3
- Verify: Brightness error -4.381449 cd/m2 (is 75.618551, should be 80.000000), White point error 0.842340 deltaE, Maximum neutral error (@ 0.807631) 4.268616 deltaE, Average neutral error 1.696932 deltaE, White drift 4.114311 deltaE

Session 2 (2026-08-07 23:11, storage folder `...F-S XYZLUT+MTX`):

- Report (pre-run): Black level 0.1017 cd/m2, White level 76.52 cd/m2 (target 80, ~4.3% low), gamma 2.15, contrast ratio 753:1, white chromaticity DE to daylight locus 0.1
- Verify: Brightness error -3.474393 cd/m2 (is 76.525607, should be 80.000000), White point error 0.151650 deltaE, Maximum neutral error (@ 0.739526) 4.014006 deltaE, Average neutral error 1.859250 deltaE, White drift 4.786826 deltaE

No clean winner between the two - session 2 has a much better white point error (0.15 vs 0.84 dE) and brightness error, but a worse average neutral error and worse white drift than session 1. Unlike the MSI comparisons, this isn't just noise deciding a winner between otherwise-good runs - both sessions share a bigger, unresolved problem: **white drift of 4.1-4.8 deltaE on both sessions**, an order of magnitude higher than anything seen on the MSI (0.2-0.7 dE) or LG monitors. Neither profile should be treated as a trustworthy final result yet.

### Root cause: Intel Display Power Saving Technologies (DPST, PSR, LACE)

Windows' own ambient-light-sensor adaptive brightness was already confirmed disabled and is not the cause. There is a whole family of Intel driver/firmware-level features, independent of and not controlled by that Windows toggle, that are documented causes of exactly this kind of drift on laptops - notably including this same laptop model (a DisplayCAL forum thread titled "Inconsistent calibration results (Dell XPS 13 & Spyder 5)" independently reports the identical problem):

- **DPST** (Display Power Saving Technology, labelled *Enhanced power saving* on this driver version, already found and disabled): analyzes on-screen content per frame and dynamically adjusts backlight and gamma to save power. Ships enabled by default. Because it reacts to displayed content, it actively fights a colorimeter measurement pass showing a constantly-changing sequence of color patches.
- **LACE** (Lighting Aware Contrast Enhancement): a genuine local tone-mapping algorithm, not just backlight dimming - dynamically remaps luminance/contrast/gamma using the laptop's ambient light sensor. The colorimeter puck sitting on/near the screen can itself interfere with the ambient sensor reading, and any room-light drift during a multi-minute measurement pass can trigger it to reapply different curves mid-session, corrupting patch-to-patch consistency. A strong suspect, arguably bigger than DPST. Disable it too.
- **PSR** (Panel Self Refresh): a link/timing power-saving feature (the panel caches the last frame so the source can idle the link on static content) - does not alter color values directly, but Intel's own i915 driver has acknowledged bugs around PSR-exit synchronization causing stale or partially-updated frames to display. A colorimeter could sample a stale/transitional frame rather than the intended one. Disable as a precaution against sync glitches even though it's not a color-value mechanism.

Disable all three before recalibrating:

1. Intel Graphics Command Center -> Settings -> System -> Power -> disable *Enhanced power saving* / *Display Power Saving* / *Adaptive Brightness* (DPST, exact label varies by driver version), *Lighting Aware Contrast Enhancement*, and *Panel Self Refresh*.
2. Older Intel Graphics Control Panel -> Power -> uncheck *Display Power-Saving Technology*.
3. If a toggle doesn't stick (a known Intel driver issue, reported for DPST specifically): registry fallback, `FeatureTestControl` DWORD under `HKLM\SYSTEM\CurrentControlSet\Control\Class\{4d36e968-e325-11ce-bfc1-08002be10318}\000x`, set bit 5 (OR with 0x10). The open-source tool [dpst-control](https://github.com/orev/dpst-control) automates this safely.
4. Also check the BIOS for a related *Dynamic Backlight Control* setting some OEMs expose separately.

Recalibrate only after confirming all three are off - neither existing profile (`shp14fa.icm` variants) should be assumed final until a session with white drift back down in the sub-1 dE range like the desktop monitors confirms the fix actually worked.

### Windows Intel driver RGB setting - not used, on purpose

Intel Graphics Command Center also exposes an RGB adjustment. Not used for this panel, for two reasons:

- Unlike external monitor OSD RGB gain (true hardware/firmware, panel-side, OS-independent), this is a Windows/Intel-driver-side software adjustment applied to the outgoing signal before the fixed physical panel - functionally similar in kind to what DisplayCAL's own VCGT already does, not a true hardware pre-correction. It doesn't carry the bit-depth-preservation benefit that made doing OSD RGB gain first worthwhile on the MSI/LG monitors.
- It's stored in Windows' own driver configuration, not in any shared hardware memory - it will not exist in a Linux boot on the same hardware, which uses a completely separate driver stack (i915/Mesa) with no visibility into Windows' settings. No cross-OS persistence benefit.

Left at neutral/default; DisplayCAL's Interactive display adjustment + VCGT handles white balance correction entirely in software instead, the same as any panel without true hardware OSD controls.

## Sources / notes

- TFT Central best-settings guide page (https://tftcentral.co.uk/best-settings/best-settings-guide-for-the-msi-mpg-321urx) has no public brightness/nits table; specific figures live in their linked video or Patreon-gated ICC database, so any brightness-to-nits mapping should be independently measured rather than trusted from secondhand notes.
- Windows Advanced Color ICC profile switching (SDR/HDR) is a real, documented OS feature, but unreliable in practice per Microsoft support threads and community reports (PC Monitors, DisplayCAL, ElevenForum forums).
- MSI MPG 321URX long-term burn-in test (OLED-info, 21 months / 5000+ hours): only 2% brightness degradation - automatic OLED Care mitigations handle most longevity concerns regardless of chosen brightness.
