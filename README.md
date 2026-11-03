# Half-Life

A caffeine tracker for Apple Watch. One glanceable face shows what you've had today and what's left of your personal limit. Logging any drink takes two taps, and the Digital Crown reviews today's drinks.

| | |
|---|---|
| **Category** | Interaction Design |
| **Year** | 2026 |
| **Tools** | HTML, CSS, JavaScript (SVG, Web Audio API), Claude, GitHub |
| **Platform** | Apple Watch (watchOS concept) |
| **Tags** | Interaction Design · Motion Design · Prototyping · Wearables · Health & Wellness · Accessibility |

Portfolio card copy is in [PROJECT.md](PROJECT.md).

## Interaction model

| Moment | What you do | What happens |
|---|---|---|
| **Glance** | Raise your wrist | The big dot-matrix number is what you've had today (*95 mg caffeine*). A ring of 72 tick lines lights up toward *your* daily limit, and the bottom line says how much of your limit is left (*76% left of 400 mg*). The face drifts from calm blue through dusty mauve to amber as you use up the day's limit. |
| **Log** | Tap **+** (top-left, the watchOS toolbar spot) | A drink list slides up. Each row shows its caffeine (e.g. *63 mg · 1 shot*) and a tiny dial previewing how much of the day it uses. Pick one and it pours into the face. |
| **Your own drinks** | **Add more** at the end of the list | Name it (optional), set the mg with − / + or the crown, and keep *Save to list* on to have it appear under **Yours** next time. Swipe a saved drink left to delete it. |
| **Undo** | Tap the dial within 4 seconds | A ring drains around the dial; a tap pours the drink back out. |
| **Review today** | Turn the Digital Crown | Step back through today's drinks, one per click. Each shows its time (*1:00 PM*), name and mg, *Drink 2 of 4 today*, and the arc and colours as they were at that moment. Rest for 3 seconds and it returns to now. |

The crown always does the obvious thing for the current screen: review today's drinks on the face, scroll in the list, and change the number on the custom screen.

Why not log on a single tap? A watch gets bumped all day, so logging takes two deliberate taps (+, then a drink). The worst a stray tap can do is open the list.

Built-in amounts are typical values (espresso 63 mg, 8 oz coffee 95 mg, 16 oz energy drink 160 mg, …). Real drinks vary, which is what *Add more* is for. Saved drinks live in the browser's local storage.

On the web: click or tap, and scroll or drag the crown. Keyboard: Tab to the buttons; arrow keys turn the crown or move through the list; Esc goes back. Respects `prefers-reduced-motion`. The demo clock runs fast (about one minute every 1.2 s).

**What's tracked:** only what you consume. Daily limits are about intake, and caffeine wearing off doesn't give allowance back, so the app doesn't show an "in your body" estimate. The count resets at midnight.

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
- **On the face:** the tick ring shows **what you've drunk today against your limit**, so it fills up and doesn't drain. Go past the limit and the ticks turn coral on a second lap. The number in the middle is the same measure: what you've had today. The bottom line gives the share of your limit that's left (*60% left of 400 mg*), then *Daily limit reached* or *Over by 108 mg*.
- **In the + list:** the first row shows *158 of 400 mg today* and opens your limit. Each drink is flagged if it's above your single-dose limit or would put you over today.
- **Saved to Health:** drinks are written to Health's Caffeine log after the 4-second undo window, so an undone drink never reaches your log.
- **Profile:** shows the values read from Health, with demo profiles (Adult, Pregnant, Teen, Lighter adult) so you can watch the limit change. *Restart demo* under the watch clears today's drinks, saved drinks and the Health setup, keeps the sound setting, and replays everything from first launch.

Apple guidelines followed here: the app uses its own heart symbol and accent colour, never the Health app's icon or pink. It uses the system font (SF on Apple devices), sentence-case watchOS list labels, Title Case buttons (*Not Now*), and region-aware units (lb and ft/in for US English). It supports Reduce Transparency and Increase Contrast. Taps have no browser highlight flash or delay.

This web prototype **simulates** Apple Health, since browsers can't read HealthKit. A watchOS build would use HealthKit's `dateOfBirth`, `bodyMass`, `height`, pregnancy and lactation, and sleep data, and read and write `dietaryCaffeine`. This is general guidance, not medical advice.

## Motion & sound

Motion principles, borrowed from premium watch-face reels and audited against Emil Kowalski's animation skills and Apple's fluid-interface guidance:

- **Quiet inner liquid.** The dark centre holds a barely tinted liquid that rises with the arc as you use up your limit. Its surface drifts on two slow waves, and after a pour it slides side to side and settles. It stays nearly colourless so the number stays crisp.
- **Tick ring that fills.** 72 radial lines circle the number. As you drink, each one fades from dim white to cold-brew amber and grows a little. The fill position is continuous, so the light glides across the ticks rather than jumping from one to the next. The leading few stand taller in a soft, glowing wave, with a long tail behind and a sharp front. The wave breathes slowly when you're calm, faster near your limit, and quickens while a drink pours. Over the limit, the ticks turn coral on a second lap.
- **Dip & bloom, never slide.** Opening the drink list sinks and blurs the face away while the list comes into focus on an iOS sheet curve (380 ms in; closing snaps back in about 220 ms). Rows bloom in 30 ms apart.
- **Staged wake-up.** On load, the glow blooms first, then the dial, the number, the text, and finally the controls.
- **Responsive, not floaty.** UI motion stays under 300 ms on a strong ease-out (`cubic-bezier(.23,1,.32,1)`). Presses give instant, subtle feedback (scale .94–.97 in 100 ms) with no overshoot, and the number's spring is critically damped.
- **Motion only where it means something.** The bottom line animates only when the *kind* of message changes, not each time *Clear by* ticks forward a minute. Hover effects are limited to real pointers.
- **Reduced motion is gentler, not off.** With `prefers-reduced-motion`, movement and scaling are dropped but opacity fades remain.
- **Cheap to render.** The colour variable that drives the calm→wired shift is scoped to the three elements that read it, so the rest of the face is never restyled; during a pour the page holds 60 fps.
- **Ripples.** Logging a drink sends three staggered rings of light outward; undo pulls them back in.
- **Cold-brew colour.** Lit ticks run from deep brew at the start of the day to honey at the leading edge.
- **Liquid pour.** The stream is many drops melted together by an SVG goo filter. Each drop stretches as gravity speeds it up, and the stream starts thin, swells, and tapers so its tail breaks off. It lands on the top of the ring, where the ticks begin, with a small splash.

Sound is kept quiet and in the background (all synthesized with Web Audio, no audio files):

- Crown detents and list rows **tick**.
- The list opens with an airy rise and closes with a fall.
- A pour is a stream of **bubbles whose pitch rises as the cup fills**, then a soft plop and a quiet two-note chime.
- Undo plays the chime in reverse. Crossing 400 mg gives a low double pulse.
- The stepper's pitch follows the number.

There's a **Sound on/off** toggle under the watch, and the setting is remembered. Browsers only allow sound after your first click or tap.

## Words on the face

One number, one meaning. The centre is what you've had (*95* / *mg caffeine*), and the bottom line is always about your limit:

| State | Bottom line |
|---|---|
| Nothing yet today | *100% left of 400 mg* |
| During the day | *60% left of 400 mg* (rounded down, so it never overstates) |
| Close to the limit (≤ 15 % or 50 mg) | *13% left of 400 mg*, highlighted |
| At the limit | *Daily limit reached* |
| Past it | *Over by 108 mg* |
| Pouring / just logged | *+200 Cold brew* → *Tap to undo* → *Saved to Health* |
| Crown review | *Drink 2 of 4 today* (time top-left, drink name under the number) |

There are no deadlines and no body-level estimates on the face.

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
