# حياة (Hayat) v2.0 — Android source

This project provides:
- Installed-app target selection.
- Overlay control panel.
- User-authorized MediaProjection screenshot capture.
- Screenshot point editor (tap to add, drag to reposition).
- Saved normalized point coordinates.
- Draggable numbered overlay points.
- One master mode: normal or long press, applied to all points.
- Repeating multi-stroke Accessibility gesture dispatch in one GestureDescription.
- Target-package foreground gating based on Accessibility window events.

## Build
Push the contents of this folder to a GitHub repository on branch `main`, then open Actions and run **Hayat Android APK**. Download the `hayat-debug-apk` artifact.

## Setup on device
1. Install the debug APK.
2. Select the target app in TouchPilot.
3. Grant overlay permission.
4. Enable TouchPilot in Android Accessibility settings.
5. Grant screen-capture permission.
6. Open the target app, use the floating panel to capture a screenshot, tap/drag points in the editor, and save.
7. Start the master control.

## Changes in v1.2
- Tap/hold strokes now use an explicit same-coordinate line, improving compatibility with Android gesture dispatch.
- Foreground-target tracking ignores unrelated accessibility content events.
- Start is blocked when there are no points or the Accessibility service is not connected.
- A rejected gesture dispatch stops the run loop and restores point touchability.
- The master normal/long-press control updates its displayed label immediately.

## Important verification status
This source has not been compiled in this environment because Android SDK/Gradle are unavailable here. The GitHub workflow is provided to perform a real build.
The OS Accessibility API accepts multiple gesture strokes in a single GestureDescription with the same start time, but actual multi-touch delivery is not guaranteed across all devices or target apps. Validate on the intended device/app.
Some apps prohibit capture or automated input. Respect their rules. This prototype does not bypass app protections.

## Changes in v1.6
- Overlay windows now opt into display-cutout areas, so chips sit exactly where gestures are sent on notched screens.
- Screenshot, saved points and gesture coordinates all use the real display size.
- Hidden overlays (during capture/editor) are also untouchable, so they no longer swallow taps in the point editor.
- Opening the keyboard no longer pauses the run loop (IME windows are ignored in foreground tracking).
- Old screenshots are deleted on each new capture instead of piling up.

## Changes in v1.7 (manual T button)
- No more automatic repeating taps. Nothing taps by itself.
- A floating round **T** button: drag it to any spot on screen.
- Tap **T** and it taps at its own position.
- From the floating panel, **إضافة نسخ من T** adds copies (T2, T3, ...). Drag each copy where you want.
- When you tap **T**, all copies tap at the same moment at their own positions. Tapping a copy directly does nothing.
- **حذف كل النسخ** removes all copies. Normal/long-press mode still applies to all.

## Changes in v1.8
- Three press types (cycle from the panel): single tap, long press, very fast repeat (no extra gap between rounds; tap T to start, tap the red circle to stop).
- "التزامن" switch: all points in one multi-touch gesture (same instant), or very quick one-after-another taps (works in apps that ignore multi-touch).
- Overlay windows use alpha 0.75 so Android 12+ does not block touches that pass through them.
- New copies get their own free slot, so copies added later never land on top of older ones.

## Changes in v1.9 (several T buttons)
- You can have up to 5 independent T buttons (T, T2, T3, ...), each with its own colour and its own copies.
- Pressing a T taps only that T and its own copies; other groups stay still.
- In the panel menu, pick a T in the top row (selected one has a thick ring), then add copies, capture a screenshot for it, or delete its copies. The "+" circle adds a new T.
- Copies are numbered by group (1.1, 1.2, 2.1, ...) and fan out next to their own T.
- Older saved data (one T plus points) is converted automatically into the first group.

## Changes in v2.0 (حياة)
- App renamed to **حياة** with a new round logo: green-to-teal disc, the word "حياة" and a small leaf. The same logo is the launcher icon (adaptive + themed/monochrome), the floating panel logo, and the main-screen header.
- Clear naming: main buttons are **T1..T5** (solid circle); copies are dashed "ghost" circles showing the owner on top and their number below (`T2 / 3`), so copies can never be mistaken for main buttons. The panel uses the same T1..T5 chips and colours.
- New **إخفاء الأزرار / إظهار الأزرار** tile: hides the look of every T and copy (they stay in place and let touches through) while the floating panel stays. Small T chips appear under the logo to press each group.
- Floating panel redesigned as a dark card with sections (الأزرار، النسخ، الضغط، الشاشة والأدوات), uniform tiles, scrolls on short screens and is kept on-screen.
- Main screen redesigned: app picker, live status for each permission (overlay, accessibility, capture), one main action button, short usage guide.
- Point editor restyled, with cancel and undo.
- Package name and class names are unchanged (`com.touchpilot`), so the build workflow keeps working.
