# Half-Life

A caffeine clock for Apple Watch. One glanceable face, one deliberate gesture to log, and the Digital Crown to look ahead.

## Interaction model

| Moment | What you do | What happens |
|---|---|---|
| **Glance** | Raise your wrist | Milligrams in your system, the coffee wedge in the dial, and *Clear by 6:48 PM* (when you'll drop under 50 mg). The face shifts from calm blue to wired amber as caffeine rises. |
| **Log** | Press and hold the dial | Coffee pours in while you hold. It snaps through sizes (Espresso 63, Coffee 95, Large 150, Cold brew 200) with a tick and a haptic at each. Let go to log the last size reached. |
| **Undo** | Tap within 4 seconds | A ring drains around the dial; a tap un-pours the drink. |
| **Time-travel** | Turn the Digital Crown | Scrub ahead to watch the caffeine decay, or back to see earlier. A mini timeline appears; let go and it springs back to now. |

Why hold-to-pour: a watch gets bumped all day, so a quick tap never logs anything. The gesture itself picks the amount, which means no menus and no small targets.

On the web, hold with the mouse or a finger. The scroll wheel (or dragging the crown) turns the crown. Keyboard: focus the screen, hold Space to pour, and use the arrow keys to turn the crown. Respects `prefers-reduced-motion`.

The demo clock runs fast (about one minute every 1.2 s), and caffeine halves roughly every 5 hours.

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
