# Half-Life

A caffeine clock for the wrist. Log a coffee and watch it pour into the dial, then slowly drain as your body metabolizes it.

- **Pour:** tap a drink (Espresso, Coffee, Matcha, Cold brew). Droplets fall from the readout into the dial, splash, and the coffee wedge springs up with a slosh.
- **Decay:** caffeine halves roughly every 5 hours. The dot-matrix readout and the wedge drain in real time (the demo clock runs fast: about one minute every 1.2 seconds).
- **Sleep-safe:** the top-right time is when you'll drop below 50 mg. Log a late espresso and watch it slide later; the moon on the timeline moves with it.
- **Mood:** the whole card shifts from calm blue to wired amber as caffeine rises. Past 400 mg (the daily guideline), the ring pulses.
- **Scrub:** drag the timeline to move through the day. Click a drink marker on the timeline to remove it.

Keyboard: Tab to the drinks or the timeline; arrow keys scrub (Shift for an hour). Respects `prefers-reduced-motion`.

A single self-contained `index.html`, no build step.

## Embedding in a portfolio

Turn on GitHub Pages (Settings → Pages → Deploy from branch → `main` / root), then:

```html
<iframe
  src="https://pallavi-k2.github.io/half-life/?embed"
  title="Half-Life caffeine clock"
  style="width:400px;height:720px;border:0;background:transparent"
  loading="lazy"></iframe>
```

`?embed` makes the page background transparent so the card sits on your site's own background.
