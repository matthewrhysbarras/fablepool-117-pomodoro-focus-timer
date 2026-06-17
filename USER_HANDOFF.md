# Pomodoro Focus Timer — User Handoff Notes

## How to use the app

1. Open `index.html` directly in any modern browser.
   - No server, build step, package install, or internet connection is required.
2. Use the controls:
   - **Start** begins or resumes the current countdown.
   - **Pause** temporarily stops the countdown without resetting it.
   - **Reset** returns the current session to its starting duration.
3. The app alternates automatically:
   - A **25-minute work session** counts down first.
   - When work ends, the completed work-session counter increases.
   - A **5-minute break session** starts next.
   - Work and break sessions continue alternating.
4. The timer is displayed in `MM:SS` format.
5. When a session ends, the browser tab title changes to make the transition visible even if the tab is in the background.

## Customizing durations

All behavior should be editable inside `index.html`.

Look in the JavaScript section near the bottom of the file for the duration values. They may appear as constants such as:

```js
const WORK_DURATION = 25 * 60;
const BREAK_DURATION = 5 * 60;
```

or similar values expressed in seconds.

To change the default durations:

```js
const WORK_DURATION = 50 * 60;  // 50 minutes
const BREAK_DURATION = 10 * 60; // 10 minutes
```

Use seconds internally so the countdown math stays simple and reliable.

Recommended examples:

| Desired duration | JavaScript value |
| --- | --- |
| 25 minutes | `25 * 60` |
| 15 minutes | `15 * 60` |
| 5 minutes | `5 * 60` |
| 90 seconds | `90` |

After editing, save `index.html` and refresh the browser tab.

## Customizing text and labels

Button labels, session names, headings, and helper text are plain HTML.

To change visible wording:

1. Open `index.html` in a text editor.
2. Search for the text you want to change, such as `Start`, `Pause`, `Reset`, `Work`, or `Break`.
3. Edit the text directly.
4. Save and refresh the browser.

Avoid changing element IDs or class names unless you also update the matching CSS and JavaScript references.

## Customizing styles

The visual design is controlled by the CSS inside `index.html`, usually in a `<style>` block near the top of the file.

Common changes:

### Change colors

Search for color values such as:

```css
background:
color:
border:
```

or CSS custom properties if present, such as:

```css
--accent-color:
--background-color:
--text-color:
```

Update those values and refresh the browser.

### Change font size

Look for styles affecting the timer display, for example:

```css
.timer {
  font-size: ...
}
```

Increase or decrease the `font-size` value to make the timer larger or smaller.

### Adjust spacing

Look for properties such as:

```css
padding:
margin:
gap:
```

These control layout spacing between the timer, buttons, and session information.

### Keep the layout responsive

When editing CSS, preserve the existing responsive behavior:

- Avoid fixed page widths that exceed small phone screens.
- Prefer `max-width` with percentage-based widths.
- Keep buttons large enough to tap comfortably on mobile.
- Test after changes by resizing the browser window.

## Maintenance guidance

This project is intentionally simple:

- Plain HTML
- Plain CSS
- Plain JavaScript
- No frameworks
- No external dependencies
- No build tools
- No package manager required

That means maintenance should stay simple too.

### Recommended edit workflow

1. Make a copy of the current working `index.html`.
2. Edit the file in a plain text editor or code editor.
3. Open `index.html` directly in a browser.
4. Test the timer manually.
5. Keep the file self-contained unless there is a clear reason to split it.

### Things to be careful with

Do not rename HTML IDs or classes without checking JavaScript and CSS.

For example, if JavaScript contains:

```js
document.getElementById("timer")
```

then the HTML must still contain:

```html
id="timer"
```

If the ID changes in one place, it must change everywhere.

### Timer logic notes

The timer should track remaining time in seconds and render it as `MM:SS`.

Useful behavior to preserve:

- Starting should not create multiple overlapping timers.
- Pausing should stop countdown updates without losing remaining time.
- Resetting should restore the current session duration.
- Completed work sessions should increase only when a work session actually finishes.
- Work and break sessions should alternate automatically.
- The tab title should update when a session changes.

### Manual test checklist after edits

After any code or style change, verify:

- `index.html` opens directly in the browser.
- The initial session is work mode.
- The timer displays `25:00` by default, unless intentionally customized.
- Start begins the countdown.
- Pause stops the countdown.
- Start after Pause resumes from the paused time.
- Reset returns the current session to its full duration.
- A finished work session increments the completed-session counter.
- A work session switches to a break session.
- A break session switches back to a work session.
- The browser tab title changes when a session ends.
- The page remains usable on a narrow/mobile-sized screen.

### Browser support

The app should work in current versions of:

- Chrome
- Edge
- Firefox
- Safari

Because it uses plain browser APIs, no compatibility tooling is required.

## Deployment notes

The app can be shared or hosted as a static file.

Valid options include:

- Opening `index.html` locally.
- Uploading `index.html` to any static web host.
- Serving the folder with a simple local web server if desired.

No build command is needed.

If hosted publicly, make sure the deployed file is the same edited `index.html` that was tested locally.

## Version-control suggestion

If this project is stored in Git, commit after each meaningful change with a clear message, for example:

```text
Adjust Pomodoro durations to 50/10
Update mobile timer spacing
Refine session-complete title text
```

This makes it easy to restore a previous working version if an edit breaks the timer.
