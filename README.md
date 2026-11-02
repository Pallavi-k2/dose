# Half-Life

A caffeine clock for Apple Watch. One glanceable face, a two-tap way to log any drink, and the Digital Crown to look ahead.

## Interaction model

| Moment | What you do | What happens |
|---|---|---|
| **Glance** | Raise your wrist | Milligrams in your system, the coffee wedge in the dial, and *Clear by 6:48 PM* (when you'll drop under 50 mg). The face shifts from calm blue to wired amber as caffeine rises. |
| **Log** | Tap **+** (top-left, the watchOS toolbar spot) | A drink list slides up. Each row shows its caffeine (e.g. *63 mg · 1 shot*) and a tiny dial previewing how much of the day it uses. Pick one and it pours into the face. |
| **Your own drinks** | **Add more** at the end of the list | Name it (optional), set the mg with − / + or the crown, and keep *Save to list* on to have it appear under **Yours** next time. Swipe a saved drink left to delete it. |
| **Undo** | Tap the dial within 4 seconds | A ring drains around the dial; a tap pours the drink back out. |
| **Time-travel** | Turn the Digital Crown | Scrub ahead to watch caffeine decay, or back to see earlier. A mini timeline appears; let go and it springs back to now. |

The crown always does the obvious thing for the current screen: time-travel on the face, scroll in the list, and change the number on the custom screen.

Why not log on a single tap? A watch gets bumped all day, so logging takes two deliberate taps (+, then a drink). The worst a stray tap can do is open the list.

Built-in amounts are typical values (espresso 63 mg, 8 oz coffee 95 mg, 16 oz energy drink 160 mg, …). Real drinks vary, which is what *Add more* is for. Saved drinks live in the browser's local storage.

On the web: click or tap, and scroll or drag the crown. Keyboard: Tab to the buttons; arrow keys turn the crown or move through the list; Esc goes back. Respects `prefers-reduced-motion`. The demo clock runs fast (about one minute every 1.2 s), and caffeine halves roughly every 5 hours.

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
