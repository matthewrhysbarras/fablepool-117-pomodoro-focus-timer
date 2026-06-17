===FILE: qa/manual-qa-checklist.md===
# Manual QA Checklist — Pomodoro Focus Timer

## Scope

This checklist verifies the Pomodoro Focus Timer as a browser app opened directly from `index.html`, with no web server, framework, package manager, or external dependency required.

The checklist covers:

- Timing accuracy for the 25-minute work timer and 5-minute break timer
- Start, Pause, Resume, and Reset behavior
- Automatic work/break session transitions
- Browser tab title updates when sessions end
- Completed work-session counting
- Responsive/mobile layout
- Keyboard usability
- Direct `file://` opening behavior
- Console/network cleanliness and edge cases

All timing checks below assume the production durations required by the project:

- Work session: `25:00`
- Break session: `5:00`

---

## Recommended test environments

Run the checklist on current stable browsers where available.

| Environment | Minimum coverage expectation |
| --- | --- |
| Desktop Chromium | Chrome or Edge on macOS, Windows, or Linux |
| Desktop Firefox | Firefox current stable |
| Desktop Safari/WebKit | Safari on macOS if available |
| Mobile Safari/WebKit | Real iPhone/iPad or browser responsive mode if real device unavailable |
| Mobile Chromium | Android Chrome or browser responsive mode if real device unavailable |
| Direct file opening | `file:///.../index.html`, not a local dev server |
| Offline behavior | Open/reload with network disabled or airplane mode after the file is available locally |

---

## Test record

Use this section during execution.

| Field | Value |
| --- | --- |
| Tester |  |
| Date |  |
| App build/source revision |  |
| Browser and version |  |
| Operating system/device |  |
| Opened via `file://` path | Yes / No |
| Network disabled during direct-file check | Yes / No |
| Console checked | Yes / No |
| Overall result | Pass / Fail |

---

## Acceptance conventions

- The timer display must be prominent and formatted as zero-padded `MM:SS`.
- Initial work time must display as `25:00`.
- Initial break time, when reached, must display as `5:00`.
- Active foreground timing should stay within approximately 1 second over a 60-second sample.
- A full 25-minute work session should transition within approximately 3 seconds of real wall-clock time.
- A full 5-minute break session should transition within approximately 2 seconds of real wall-clock time.
- The timer must never display negative values such as `-1:59` or `00:-1`.
- Completed work sessions must increment only when a work session completes.
- Reset is treated as an app-level reset: stopped work mode, `25:00`, completed work sessions `0`, and a neutral/default browser title.
- If an implementation intentionally chooses different Reset semantics, that product decision should be explicit in the UI or project documentation before QA sign-off.

---

## Pre-test setup

1. Start from a fresh local copy of the app files.
2. Open the app by double-clicking `index.html`, dragging it into a browser window, or using the browser’s **File > Open File** command.
3. Confirm the address bar uses a `file://` URL.
4. Open browser DevTools Console.
5. When checking external dependency behavior, also open the Network panel and reload.
6. Use a phone stopwatch, system clock, or browser stopwatch for timing checks.
7. Close duplicate copies of the app unless the test case explicitly covers multi-tab behavior.
8. For final sign-off, run at least one real-duration work-to-break-to-work cycle using the production `25:00` and `5:00` durations.

Optional acceleration for exploratory testing only:

- A local scratch copy may temporarily shorten durations to quickly repeat edge cases.
- Do not use modified durations as the only evidence for release sign-off.
- Restore and verify the required `25:00` / `5:00` production durations before final acceptance.

---

## A. Direct file-opening and dependency independence

| ID | Steps | Expected result | Result / notes |
| --- | --- | --- | --- |
| DF-01 | Open `index.html` directly from the filesystem. Confirm the URL begins with `file://`. | App loads successfully without a local server. Timer UI is visible and interactive. |  |
| DF-02 | Reload the page while still using the `file://` URL. | App reloads cleanly into its initial state. |  |
| DF-03 | Disable network access or turn on airplane mode, then reload the already-local `index.html`. | App still loads and works. No external CDN, font, framework, image, script, or stylesheet is required. |  |
| DF-04 | Inspect DevTools Console after initial load. | No uncaught JavaScript errors, missing-file errors, module/CORS errors, or blocked external dependency errors. |  |
| DF-05 | Inspect DevTools Network panel during reload. | No remote network requests are required. If local CSS/JS assets exist, they load via relative local file paths only. |  |
| DF-06 | Open the file from a directory path containing spaces, for example `Pomodoro Timer/index.html`. | App still loads correctly; local relative paths are not broken by spaces. |  |
| DF-07 | Open a second independent browser tab with the same `index.html` file. | Each tab has its own timer state. Starting, pausing, or resetting one tab does not corrupt the other. |  |

---

## B. Initial display and base UI

| ID | Steps | Expected result | Result / notes |
| --- | --- | --- | --- |
| UI-01 | Open or reload the app. | Initial timer displays `25:00` prominently. |  |
| UI-02 | Inspect the visible session/mode label. | The UI clearly indicates the app is in a work/focus session. |  |
| UI-03 | Inspect the completed work-session counter. | Counter starts at `0`. |  |
| UI-04 | Inspect available controls. | Start, Pause, and Reset controls are visible and understandable. |  |
| UI-05 | Inspect timer formatting at initial state. | Timer uses zero-padded `MM:SS`; for example `25:00`, not `25:0` or raw seconds. |  |
| UI-06 | Inspect the browser tab title immediately after load. | Title is neutral and readable, such as the app name or current Pomodoro state. It is not stuck on an old completion message. |  |
| UI-07 | Confirm the timer is stopped immediately after load. Wait 10 seconds without pressing Start. | Display remains `25:00`; completed count remains `0`. |  |
| UI-08 | Resize the desktop window narrower and wider. | Core timer, mode, counter, and buttons remain visible and usable. |  |

---

## C. Timing accuracy

| ID | Steps | Expected result | Result / notes |
| --- | --- | --- | --- |
| T-01 | From initial `25:00`, press Start and start an external stopwatch at the same time. After 60 seconds, inspect the display. | Timer is approximately `24:00`, allowing about 1 second of visual/update tolerance. |  |
| T-02 | Continue the same running work session until 5 minutes of wall-clock time have elapsed. | Timer is approximately `20:00`, allowing about 2 seconds of tolerance. |  |
| T-03 | Watch the display for at least 20 consecutive seconds while running. | Seconds decrement smoothly once per second. No obvious double-speed countdown, skipped multi-second jumps in the active foreground, or frozen display. |  |
| T-04 | Let a full work session run from `25:00` to completion in the foreground. | Transition to break occurs around 25 minutes of wall-clock time, within approximately 3 seconds. |  |
| T-05 | Let a full break session run from `5:00` to completion in the foreground. | Transition back to work occurs around 5 minutes of wall-clock time, within approximately 2 seconds. |  |
| T-06 | While a session is running, switch to another tab or app for 2 minutes, then return. | Timer display reflects elapsed wall-clock time and does not simply pause because the tab was hidden. A small browser-throttling delay on visual update is acceptable once the tab is focused again. |  |
| T-07 | During a running session, leave the app untouched for at least 10 minutes. | Timer remains accurate enough for practical use, does not stop unexpectedly, and does not accumulate large drift. |  |
| T-08 | Observe the moment of transition from `00:01` through session end. | Timer never displays a negative time. It should transition cleanly to the next session. |  |
| T-09 | If feasible, lock the device or let the screen sleep for several minutes during a running timer, then unlock. | App recovers gracefully. It should either reflect elapsed time correctly or transition to the proper current session without errors or negative values. Record browser/device behavior because mobile OS background suspension can vary. |  |

---

## D. Start, Pause, Resume, and Reset behavior

| ID | Steps | Expected result | Result / notes |
| --- | --- | --- | --- |
| B-01 | From initial state, press Start once. | Work countdown begins from `25:00`. |  |
| B-02 | Press Start repeatedly while the timer is already running. | App does not create duplicate intervals.