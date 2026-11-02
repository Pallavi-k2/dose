# Half-Life

A caffeine clock for Apple Watch. One glanceable face, a two-tap way to log any drink, and the Digital Crown to look ahead.

## Interaction model

| Moment | What you do | What happens |
|---|---|---|
| **Glance** | Raise your wrist | The caffeine in you now (dot-matrix *mg caffeine*) sits inside a glowing cold-brew arc that fills with what you've drunk today toward *your* limit, with *158 / 400 mg* underneath. The face drifts from calm blue through dusty mauve to amber as caffeine rises. |
| **Log** | Tap **+** (top-left, the watchOS toolbar spot) | A drink list slides up. Each row shows its caffeine (e.g. *63 mg · 1 shot*) and a tiny dial previewing how much of the day it uses. Pick one and it pours into the face. |
| **Your own drinks** | **Add more** at the end of the list | Name it (optional), set the mg with − / + or the crown, and keep *Save to list* on to have it appear under **Yours** next time. Swipe a saved drink left to delete it. |
| **Undo** | Tap the dial within 4 seconds | A ring drains around the dial; a tap pours the drink back out. |
| **Time-travel** | Turn the Digital Crown | Scrub ahead to watch caffeine decay, or back to see earlier. A mini timeline appears; let go and it springs back to now. |

The crown always does the obvious thing for the current screen: time-travel on the face, scroll in the list, and change the number on the custom screen.

Why not log on a single tap? A watch gets bumped all day, so logging takes two deliberate taps (+, then a drink). The worst a stray tap can do is open the list.

Built-in amounts are typical values (espresso 63 mg, 8 oz coffee 95 mg, 16 oz energy drink 160 mg, …). Real drinks vary, which is what *Add more* is for. Saved drinks live in the browser's local storage.

On the web: click or tap, and scroll or drag the crown. Keyboard: Tab to the buttons; arrow keys turn the crown or move through the list; Esc goes back. Respects `prefers-reduced-motion`. The demo clock runs fast (about one minute every 1.2 s), and caffeine halves roughly every 5 hours.

## Your safe limit (Apple Health)

The limit adapts to you instead of assuming everyone's is 400 mg:

| Who | Daily limit | Source |
|---|---|---|
| Healthy adults | 400 mg, and up to 200 mg at once (≈ 3 mg per kg) | EFSA 2015, FDA |
| Pregnant or breastfeeding | 200 mg | EFSA |
| Teens 12–17 | 3 mg per kg, up to 100 mg | EFSA, AACAP |
| Under 12 | Not recommended | AAP |

Height is read but deliberately not used, because none of this guidance depends on it. The app says so rather than pretending otherwise.

- **First launch:** a short *Connect Apple Health* screen explains why the data is needed, then a permission sheet modelled on the system one. Every type starts **off**, with *Turn On All* and individual switches. It then shows *Your daily limit*.
- **Partial consent is respected.** Anything not shared falls back to the standard assumption, the profile shows it as *Not shared*, and the limit explains what to share for a more personal one (*Share Date of Birth for a more personal limit*). Without Sleep there's no bedtime line, and without write access nothing is saved to Health.
- **On the face:** the arc shows **what you've drunk today against your limit**, so it fills up and doesn't drain. Go past the limit and a second, coral lap starts over the first, like an Activity ring. The number in the middle shows what's in your body right now. The bottom line reads *158 / 400 mg*, or *Over your limit*.
- **In the + list:** the first row shows *158 of 400 mg today* and opens your limit. Each drink is flagged if it's above your single-dose limit or would put you over today.
- **Saved to Health:** drinks are written to Health's Caffeine log after the 4-second undo window, so an undone drink never reaches your log.
- **Profile:** shows the values read from Health, with demo profiles (Adult, Pregnant, Teen, Lighter adult) so you can watch the limit change. *Health setup* under the watch replays the onboarding.

Apple guidelines followed here: the app uses its own heart symbol and accent colour, never the Health app's icon or pink. It uses the system font (SF on Apple devices), sentence-case watchOS list labels, Title Case buttons (*Not Now*), and region-aware units (lb and ft/in for US English). It supports Reduce Transparency and Increase Contrast. Taps have no browser highlight flash or delay.

This web prototype **simulates** Apple Health, since browsers can't read HealthKit. A watchOS build would use HealthKit's `dateOfBirth`, `bodyMass`, `height`, pregnancy and lactation, and sleep data, and read and write `dietaryCaffeine`. This is general guidance, not medical advice.

## Motion & sound

Motion principles, borrowed from premium watch-face reels and audited against Emil Kowalski's animation skills and Apple's fluid-interface guidance:

- **One ring, nothing extra.** The centre is just the number and the arc. The arc's glow breathes slowly when you're calm and faster as you near your limit, and it quickens while a drink pours.
- **Dip & bloom, never slide.** Opening the drink list sinks and blurs the face away while the list comes into focus on an iOS sheet curve (380 ms in; closing snaps back in about 220 ms). Rows bloom in 30 ms apart.
- **Staged wake-up.** On load, the glow blooms first, then the dial, the number, the text, and finally the controls.
- **Responsive, not floaty.** UI motion stays under 300 ms on a strong ease-out (`cubic-bezier(.23,1,.32,1)`). Presses give instant, subtle feedback (scale .94–.97 in 100 ms) with no overshoot, and the number's spring is critically damped.
- **Motion only where it means something.** The bottom line animates only when the *kind* of message changes, not each time *Clear by* ticks forward a minute. Hover effects are limited to real pointers.
- **Reduced motion is gentler, not off.** With `prefers-reduced-motion`, movement and scaling are dropped but opacity fades remain.
- **Cheap to render.** The colour variable that drives the calm→wired shift is scoped to the three elements that read it, so the rest of the face is never restyled; during a pour the page holds 60 fps.
- **Ripples.** Logging a drink sends three staggered rings of light outward; undo pulls them back in.
- **Cold-brew arc.** The arc runs from deep brew at its start to honey at its glowing tip, and glides to its new length with no bounce.
- **Liquid pour.** The stream is many drops melted together by an SVG goo filter. Each drop stretches as gravity speeds it up, and the stream starts thin, swells, and tapers so its tail breaks off. It lands on the top of the ring, where the arc begins, with a small splash.

Sound is kept quiet and in the background (all synthesized with Web Audio, no audio files):

- Crown detents and list rows **tick**.
- The list opens with an airy rise and closes with a fall.
- A pour is a stream of **bubbles whose pitch rises as the cup fills**, then a soft plop and a quiet two-note chime.
- Undo plays the chime in reverse. Crossing 400 mg gives a low double pulse.
- The stepper's pitch follows the number.

There's a **Sound on/off** toggle under the watch, and the setting is remembered. Browsers only allow sound after your first click or tap.

## Words on the face

As little text as possible, and it always names caffeine: *mg caffeine* under the number, *158 / 400 mg* for today, *+200 Cold brew* while pouring, *Tap to undo*, *Removed Cold brew*, *Saved to Health*, *Over your limit*, and *Tap + to add* when the day is empty. There are no deadlines on the face.

Contrast: nothing is layered over the gradient. Instead, the clock, the *Ahead/Earlier* label and the bottom line each check the colour actually behind them as the face shifts, and switch between white and deep ink to whichever reads better. A tight shadow hugs each letter. Worst case is 4.5:1 or better before counting that shadow. The rest of the UI was checked pair by pair: secondary greys are at least 6:1, switches that are off get a visible outline, and the arc keeps at least 3:1 against its dark track. The + sits on its own small dark button.

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
