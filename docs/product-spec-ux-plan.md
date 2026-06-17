# Pomodoro Focus Timer — Product Spec and UX Plan

## Purpose

Build a lightweight single-page Pomodoro timer that runs entirely in the browser by opening `index.html` directly. The app helps users alternate between focused 25-minute work sessions and 5-minute breaks, tracking completed work sessions with a clean, responsive interface.

## Technical Constraints

- Use plain HTML, CSS, and JavaScript only.
- No frameworks, build tools, package managers, or external dependencies.
- Must work from the local filesystem by directly opening `index.html`.
- All behavior should be contained in static project files.

## Core Timer Model

### Session Types

The app alternates between two session types:

| Session Type | Duration | Completion Effect |
| --- | ---: | --- |
| Work | 25:00 | Increment completed work-session count, then switch to Break |
| Break | 05:00 | Do not increment completed count, then switch to Work |

### Timer States

The app should support these states:

1. **Idle**
   - Initial state when the app loads.
   - Timer displays `25:00`.
   - Current session type is Work.
   - Completed work sessions display `0`.
   - Start is enabled.
   - Pause is disabled or visually inactive.
   - Reset is enabled.

2. **Running**
   - Countdown decreases once per second.
   - Current session label indicates either Work or Break.
   - Start is disabled or visually inactive.
   - Pause is enabled.
   - Reset is enabled.

3. **Paused**
   - Countdown is stopped at the current remaining time.
   - Current session type does not change.
   - Start resumes from the paused time.
   - Pause is disabled or visually inactive.
   - Reset is enabled.

4. **Session Transition**
   - When the countdown reaches `00:00`, the current session completes immediately.
   - The app switches automatically to the next session type.
   - The next session begins automatically without requiring the user to press Start again.
   - The browser tab title updates to announce the new session.
   - The visible timer resets to the new session duration and continues counting down.

## Work/Break Alternation Behavior

- The app starts with a Work session.
- Work and Break sessions alternate indefinitely:
  - Work → Break → Work → Break.
- A completed work-session count increments only when a Work session reaches `00:00` naturally.
- Reset does not count as a completed session.
- Pausing does not affect the completed-session count.
- Break completions never increment the completed-session count.

## Button Behavior

### Start Button

- From Idle:
  - Begins the current Work countdown from `25:00`.
  - App state becomes Running.
- From Paused:
  - Resumes the current countdown from its remaining time.
  - App state becomes Running.
- From Running:
  - Has no effect, or is disabled to prevent duplicate timers.

### Pause Button

- From Running:
  - Stops the countdown without changing the remaining time.
  - App state becomes Paused.
- From Idle or Paused:
  - Has no effect, or is disabled.

### Reset Button

- From any state:
  - Stops any active countdown.
  - Restores the app to the initial Work session.
  - Sets timer display to `25:00`.
  - Sets completed work sessions to `0`.
  - Sets app state to Idle.
  - Restores the browser title to the default title.

## Timer Display Rules

- The timer must be displayed prominently in `MM:SS` format.
- Minutes and seconds must always use two digits:
  - `25:00`
  - `05:00`
  - `00:09`
- The timer should never display negative values.
- The visible display should update immediately when:
  - The page loads.
  - Start is pressed.
  - Pause is pressed.
  - Reset is pressed.
  - A session transitions.

## Completed Work Sessions Counter

- Display a clear label such as: `Completed work sessions: 0`.
- Initial value is `0`.
- Increment by `1` only when a Work session reaches `00:00`.
- Do not increment when:
  - A Break session ends.
  - The user presses Reset.
  - The user pauses or resumes.
  - The timer is manually reset before completion.

## Browser Tab Title Rules

Default title:

- `Pomodoro Focus Timer`

When a Work session completes and Break begins:

- Update the title to: `Break time! — Pomodoro Focus Timer`

When a Break session completes and Work begins:

- Update the title to: `Focus time! — Pomodoro Focus Timer`

When Reset is pressed:

- Restore the title to: `Pomodoro Focus Timer`

The browser title update is the required completion signal. Audio alerts and desktop notifications are out of scope for the initial version.

## Layout and Visual Design

### Overall Direction

- Clean, calm, distraction-free design.
- Center the main timer card both visually and structurally.
- Use generous spacing, high contrast, and large readable text.

### Main UI Elements

The page should include:

1. App heading: `Pomodoro Focus Timer`
2. Current session label:
   - `Work Session`
   - `Break Session`
3. Prominent timer display.
4. Completed work sessions counter.
5. Controls:
   - Start
   - Pause
   - Reset

### Responsive Layout Notes

- Use a mobile-first layout.
- On small screens:
  - Stack content vertically.
  - Buttons may stack or wrap with comfortable spacing.
  - Timer should remain the dominant visual element.
- On wider screens:
  - Keep the timer card centered.
  - Allow buttons to sit in a horizontal row.
- Avoid fixed widths that cause horizontal scrolling.
- Use relative units where appropriate.
- Ensure controls remain easy to tap on touch devices.

## Accessibility Considerations

- Use semantic HTML:
  - Main content inside `<main>`.
  - Buttons implemented as real `<button>` elements.
- Provide clear visible button text.
- Ensure all controls are keyboard accessible.
- Maintain visible focus styles for buttons.
- Use sufficient color contrast for text, buttons, and backgrounds.
- Do not rely on color alone to communicate Work vs Break state; also use text labels.
- Announce timer/session changes accessibly:
  - The timer or status area should use an appropriate `aria-live` region.
  - Session transition text should be readable by assistive technology.
- Avoid rapid or distracting animation.
- Respect users who prefer reduced motion if any transitions are added.
- Use touch-friendly control sizes, ideally at least 44px high.
- The interface should remain understandable at browser zoom levels up to at least 200%.

## Acceptance Criteria

The implementation is complete when:

- Opening `index.html` directly in a browser shows the timer app without setup.
- The initial timer displays `25:00`.
- Start begins a 25-minute Work countdown.
- Pause stops the active countdown without resetting it.
- Start after Pause resumes the countdown.
- Reset restores the initial state and clears completed sessions.
- Work sessions automatically transition to 5-minute Break sessions.
- Break sessions automatically transition back to 25-minute Work sessions.
- Completed work sessions increment only after Work sessions finish.
- Browser tab title changes when sessions end.
- The layout is clean and usable on mobile and desktop screens.
- The app uses no external dependencies.
