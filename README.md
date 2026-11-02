# Half-Life

A caffeine clock for Apple Watch. One glanceable face, a two-tap way to log any drink, and the Digital Crown to look ahead.

## Interaction model

| Moment | What you do | What happens |
|---|---|---|
| **Glance** | Raise your wrist | Milligrams in your system, a wedge of soft caramel liquid in a dark glass dial, and *Clear by 6:48 PM* (when you'll drop under 50 mg). The face drifts from calm blue through dusty mauve to muted amber as caffeine rises. |
| **Log** | Tap **+** (top-left, the watchOS toolbar spot) | A drink list slides up. Each row shows its caffeine (e.g. *63 mg · 1 shot*) and a tiny dial previewing how much of the day it uses. Pick one and it pours into the face. |
| **Your own drinks** | **Add more** at the end of the list | Name it (optional), set the mg with − / + or the crown, and keep *Save to list* on to have it appear under **Yours** next time. Swipe a saved drink left to delete it. |
| **Undo** | Tap the dial within 4 seconds | A ring drains around the dial; a tap pours the drink back out. |
| **Time-travel** | Turn the Digital Crown | Scrub ahead to watch caffeine decay, or back to see earlier. A mini timeline appears; let go and it springs back to now. |

The crown always does the obvious thing for the current screen: time-travel on the face, scroll in the list, and change the number on the custom screen.

Why not log on a single tap? A watch gets bumped all day, so logging takes two deliberate taps (+, then a drink). The worst a stray tap can do is open the list.

Built-in amounts are typical values (espresso 63 mg, 8 oz coffee 95 mg, 16 oz energy drink 160 mg, …). Real drinks vary, which is what *Add more* is for. Saved drinks live in the browser's local storage.

On the web: click or tap, and scroll or drag the crown. Keyboard: Tab to the buttons; arrow keys turn the crown or move through the list; Esc goes back. Respects `prefers-reduced-motion`. The demo clock runs fast (about one minute every 1.2 s), and caffeine halves roughly every 5 hours.

## Motion & sound

Motion principles, borrowed from premium watch-face reels:

- **Light lives at the edges.** Three blurred ribbons of light drift around the dial's rim at different speeds and blend additively. They flow faster the more caffeine you've had, and surge while a drink pours.
- **Dip & bloom, never slide.** Opening the drink list sinks and blurs the face away while the list comes into focus. Rows bloom in one after another, about 38 ms apart.
- **Staged wake-up.** On load, the glow blooms first, then the dial, the number, the text, and finally the controls.
- **Long, smooth deceleration.** Everything eases with expo-out (`cubic-bezier(.16,1,.3,1)`), and the number's spring is tuned for glide with only a slight settle.
- **Ripples.** Logging a drink sends three staggered rings of light outward; undo pulls them back in.
- **Caramel liquid.** The wedge is a soft honey-caramel, lighter at the core and deeper at the rim. Soft heat-haze bands sway through it, and its colours breathe slowly. When it moves it sloshes, and it springs into place with no bounce.
- **Liquid pour.** The stream is many drops melted together by an SVG goo filter. Each drop stretches as gravity speeds it up, and the stream starts thin, swells, and tapers so its tail breaks off. A small splash and pool form where it lands.

Sound is kept quiet and in the background (all synthesized with Web Audio, no audio files):

- Crown detents and list rows **tick**.
- The list opens with an airy rise and closes with a fall.
- A pour is a stream of **bubbles whose pitch rises as the cup fills**, then a soft plop and a quiet two-note chime.
- Undo plays the chime in reverse. Crossing 400 mg gives a low double pulse.
- The stepper's pitch follows the number.

There's a **Sound on/off** toggle under the watch, and the setting is remembered. Browsers only allow sound after your first click or tap.

Contrast: nothing is layered over the gradient. Instead, the clock, the *Ahead/Earlier* label and the bottom line each check the colour actually behind them as the face shifts, and switch between white and deep ink to whichever reads better. A tight shadow hugs each letter. Worst case is about 4.5:1 before counting that shadow. The + sits on its own small dark button.

A single self-contained `index.html`, no build step.

## Embedding in a portfolio

Turn on GitHub Pages (Settings → Pages → Deploy from branch → `main` / root), then:

```html
<iframe
  src="https://pallavi-k2.github.io/half-life/?embed"
  title="Half-Life caffeine clock for Apple Watch"
  style="width:420px;height:820px;border:0;background:transparent"
  loading="lazy"></iframe>
```

`?embed` makes the page background transparent so the watch sits on your site's own background.
