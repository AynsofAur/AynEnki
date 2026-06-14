# Malikeye / AynEnki — Streaming Site & Reactive Neural Avatar

The master site for the streaming brand. Hosts the public-facing pages (home, portfolio, shop, commissions, about) plus a full suite of OBS Browser Source overlays driven by a face-tracked, voice-reactive neural network avatar.

Live: <https://aynsofaur.github.io/AynEnki/>
Repo: <https://github.com/AynsofAur/AynEnki>

---

## File map

- `index.html` — master site, admin panel, every scene overlay (Malikeye SVG-eye scenes, AynEnki gold-ring scenes, **Neural HUD scenes**, game scene overlay, quote sheet)
- `neural-avatar.html` — standalone Three.js reactive avatar. Loaded as its own OBS Browser Source, or embedded in iframe inside the neural scenes
- `README.md` — this file

Both HTML files are self-contained (no build step). GitHub Pages serves them as static files.

---

## Quick start: getting the avatar live in OBS

This is the absolute minimum. Skip nothing.

1. **Launch OBS with the camera/mic permission flag.** Right-click your OBS shortcut → Properties → Shortcut tab → in the Target field, append ` --use-fake-ui-for-media-stream` after the executable path. Always launch OBS from this shortcut. Without this, Browser Source camera permissions get silently denied.

2. **In each Browser Source you create**: turn ON both "Shutdown source when not visible" and "Refresh browser when scene becomes active". The first releases cam/mic between scenes; the second triggers the entrance fade animations each scene switch.

3. **Set OBS Settings → Scene Transitions → default = Fade, duration = 450ms.** Pair with the entrance animations baked into the neural scenes for smooth cross-dissolves.

4. **Disable any virtual cameras you don't want.** OBS Browser Source's CEF will happily grab the first webcam it sees, which is often a VTube Studio / NVIDIA Broadcast / Snap Camera virtual device instead of your real webcam. Disable in Device Manager → Cameras, or use `?camera=brio` (or similar substring of your real camera's name) in the URL to force selection.

---

## Scene URLs — what each one is for

Public site routes (visible nav):
- `#home`, `#portfolio`, `#shop`, `#commissions`, `#about`

Admin (password-gated): `#admin`

Original Malikeye SVG-eye overlays: `#starting-soon`, `#brb`, `#ending`, `#just-chatting`, `#game-scene`, `#game-goals`, `#game-info-bar`

AynEnki gold-ring overlays: `#ae-starting-soon`, `#ae-brb`, `#ae-ending`, `#ae-overlay`

**Neural Network HUD overlays (current generation):**
- `#neural-starting` — countdown timer scene, "Initializing the network"
- `#neural-brb` — "Be Right Back" with rotating flicker phrases
- `#neural-ending` — "Thank you for watching" closing scene
- `#neural-tech` — "SIGNAL LOST" / Technical Difficulties (glitch shake, red HUD)
- `#neural-game` — corner-overlay gameplay scene (transparent middle for game capture)

Standalone avatar (for OBS Browser Sources that aren't inside a scene HUD):
- `neural-avatar.html?obs=1&morph=6&theme=3&density=100&intensity=high` — full showcase

Stinger transition (for screen recording into MP4):
- `neural-avatar.html?stinger=1` — looping 1s collapse-then-burst animation

---

## URL parameters reference — `neural-avatar.html`

All params are query-string. Multiple combine with `&`.

### Visual presets
- `morph=N` — formation 0-13 (Sphere/Helix/Fractal/Torus/Lattice/Bicone/**Tree of Life**/Eye/Skull/Liquid/Construct/Construct Hybrid/Tree-Construct/**Sephiroth**). Default: 0
- `theme=N` — palette 0-3 (Purple Nebula / Sunset Fire / Ocean Aurora / **Malikeye**). Default: 0
- `density=N` — node density % (30-100). Default: 100
- `intensity=low|med|high` — overall effect intensity. `low` is leaner for older hardware

### Canvas framing (v2)
- `framex=N` / `framey=N` — shift the rendered canvas by N px (-400 to 400) inside the OBS frame
- `framezoom=N` — zoom the canvas % (40-200). Default 100
- `adaptive=0` — disable adaptive resolution (on by default: render scale steps down 100→85→70→60% to hold ~58fps)

### Color overrides (all hex without #)
- `inner=hex` — inner shell color
- `mid=hex` — middle shell color
- `outer=hex` — outer shell color
- `sparkle=hex` — pulse color
- `bloomtint=hex` — bloom post-process color
- `nucleus=hex` — bright central core sphere color (the pulsing dot)
- `halo=hex` — soft halo around nucleus
- `veil1c=hex` / `veil2c=hex` / `veil3c=hex` — veil ring colors, outer/mid/inner (v2; defaults a8b4ff / e8b84b / fff4d0)

### Veil glyphs (ARG layer)
- `veil1=set` — outer veil glyph set (hebrew/runes/greek/arabic/sanskrit/alchemy/astro/latin/mixed)
- `veil2=set`, `veil3=set` — middle and inner veils
- `veil1text=CUSTOM`, `veil2text=...`, `veil3text=...` — replace random glyphs with your custom string
- `veillegible=1` — disable glitch flicker so glyphs are fully readable
- `glitchset=set` — glyph set for background noise glitches

### Background / chroma keying
- `bg=dark|green|blue|magenta|#hex` — overrides transparent. Use `green` for chroma-key workflows

### Mic / mouth calibration
- `gain=N` — voice gain % (50-500). Default 100. Bump if mic is quiet in OBS
- `scale=N` — mouth-driven scale sensitivity % (0-500). Default 100. Set 0 to kill mouth-driven growth entirely

### Camera selection
- `camera=substring` — force a specific webcam by partial label match (case-insensitive). Example: `camera=brio` or `camera=c920`

### Operating modes
- `obs=1` — OBS clean mode (hides setup panel, control buttons, FPS counter). **Required for any OBS Browser Source** unless you want viewers seeing your panels
- `aur=0` — hide AinSophAur veil rings
- `eye=1` — show the Eye-of-Horus center sigil (hidden by default)
- `stars=0` — hide the starfield background
- `test=1` — test mode, simulates voice/face without webcam/mic. For previewing
- `debug=1` — show live diagnostic readout (voice/mouth/yaw/pitch values) and the cam preview

### Stinger transition mode
- `stinger=1` — scripted collapse→burst loop, no cam/mic, designed for MP4 recording
- `stingerdur=N` — cycle duration in seconds (0.4-3.0). Default 1.0
- `stingertint=hex` — color of the midpoint flash (default cyan-white)

---

## Admin panel

Reach via `#admin`. Login is gated by `state.password` (default empty on first visit; set one in Tools → Security · Data).

**Tabs (top of admin):**
- **Streaming** — scene layouts, OBS URL builder, AynEnki content, overlay text, stream pages launch
- **Identity** — brand, social handles, stream schedule
- **Content** — portfolio pieces, shop items, commissions
- **Tools** — embers settings, import/export config, password, reset

Tab + each section's collapsed state persist in localStorage.

**SAVE ALL CHANGES** at the bottom saves *everything*, including the URL builder and neural scene fields. The per-section "Save" buttons are convenience shortcuts only.

---

## Hidden quirks & gotchas — read before debugging

### Caching

GitHub Pages serves with aggressive cache headers. After any push:
- Hard refresh with `Ctrl + Shift + R` (Windows) or `Cmd + Shift + R` (Mac)
- Or open in an incognito window for a clean test
- Or open DevTools → Network → check "Disable cache"

If a new feature doesn't appear after pushing, this is almost always why.

### localStorage isolation between browsers and OBS

OBS Browser Source has its own CEF process with its own localStorage. **It does not share state with your regular browser.** When you save settings in admin (in your regular browser), OBS doesn't see those changes.

The fix that's already built-in: anything user-tunable propagates to OBS via URL parameters, not localStorage. The admin URL builder + neural scene URLs encode all your tuned values directly. So OBS sources load the values from the URL, no shared state needed.

If you ever wonder "why doesn't OBS show my admin changes" — the value either needs to be saved → URL builder URL rebuilt → URL pasted into OBS, or the change needs to propagate through `state.neural` / `state.avatarBuilder` which the neural scene init reads when generating the iframe URL.

### Camera & mic permissions in OBS

`getUserMedia` requires HTTPS. GitHub Pages serves HTTPS, so that's fine.

But OBS's CEF browser:
- Has `autoGainControl` **OFF by default** (regular Chrome has it ON). The avatar explicitly requests AGC: ON to compensate. Without this, your mic reads way quieter in OBS than in a Chrome tab.
- Does not honor `default_content_setting_values` in its Preferences JSON reliably (we tried).
- DOES honor command-line flags. Hence `--use-fake-ui-for-media-stream` on the OBS shortcut Target field.

### Virtual cameras intercepting your real webcam

If you've installed VTube Studio, NVIDIA Broadcast, Snap Camera, ManyCam, OBS Virtual Camera, Streamlabs, Elgato — any of them registers a virtual webcam device. CEF will sometimes grab the virtual cam before your real one and you'll get either a test pattern or nothing.

The smart camera picker in `neural-avatar.html` actively skips known virtual cameras by name pattern: `vtube`, `vts virtual`, `obs virtual`, `nvidia broadcast`, `snap camera`, `manycam`, `xsplit`, `streamlabs`, `elgato virtual`, `droidcam`, `epoccam`, `iriun`. If your virtual cam isn't on that list, add `?camera=substring` to the URL to force-select your real one. Or disable the virtual cam in Device Manager.

The console logs the picked camera on every load:
```
[neural-avatar] available cameras: HD Pro Webcam C920 | VTubeStudioCamera
[neural-avatar] picked camera: HD Pro Webcam C920
```

### OBS audio filters don't apply to Browser Source mic input

If you've added Noise Suppression, Compressor, EQ to your OBS mic source — those filters apply to your stream output, NOT to the audio that Browser Sources read via `getUserMedia`. The neural avatar gets raw mic input from the OS, not OBS-processed.

That's why we have `gain=` URL param — to multiply the avatar's raw mic input independently of your OBS filter chain.

### The Awaken button auto-clicks in OBS mode

When the URL has `?obs=1`, the avatar auto-triggers the Awaken button 1 second after load. So the cam/mic flow starts automatically. **However**, the page still needs a user gesture for permission grants — which is supplied by the `--use-fake-ui-for-media-stream` launch flag (it pre-grants).

If you load the avatar in a regular Chrome tab WITHOUT `?obs=1`, you must click Awaken yourself to grant permissions.

### Removing `?obs=1` to debug, and the panel showing on stream

If you strip `?obs=1` to make the setup panel visible (for granting permissions in incognito, etc.) and forget to add it back, the setup panel will show on stream. The recent fix forces the panel to fully hide (opacity 0 + pointer-events none) shortly after Awaken even without `?obs=1`, but `?obs=1` is still the correct setting for production OBS sources.

### Scene fade-in vs OBS transitions

The neural HUD scenes have CSS entrance animations built in (corners → text → status bar arrive in a staggered cadence over ~1 second on every page load). For these to replay on every scene switch in OBS, the Browser Source must have **"Refresh browser when scene becomes active" ON**.

If you turn that off, the entrance animations only play once on first source load, and subsequent scene switches will hard-cut to the already-rendered state.

The OBS Fade transition (Settings → Scene Transitions) is independent and stacks on top — it cross-dissolves between the outgoing and incoming source pixels. Both effects together give the polished "neural network materializing" feel.

### Stinger transition workflow

`neural-avatar.html?stinger=1` runs a scripted animation loop with NO cam/mic, designed for screen-recording into an MP4. The midpoint of each cycle (at 500ms in a 1000ms cycle) is the moment of maximum coverage — this is where you set OBS's Transition Point.

Workflow:
1. Open `?stinger=1` in fullscreen Chrome (F11)
2. `Win + Alt + R` to start Game Bar recording
3. Wait 3 seconds (captures 3 loops)
4. Stop recording
5. Trim to one clean cycle in any editor (or via ffmpeg)
6. OBS → Settings → Scene Transitions → + → Stinger → load your MP4 → Transition Point = `500ms` (or half your cycle duration)

### `?obs=1` and transparency in iframes

The neural avatar iframe embedded in the neural scenes uses `allowtransparency="true"`. The avatar itself respects `body.transparent` class which sets the page background to actually transparent. `?obs=1` automatically sets this. Without it, an embedded iframe shows a dark background where there should be game capture or HUD.

### Per-scene bloom tint always overrides URL builder

The neural scenes (`starting`, `brb`, `ending`, `tech`, `game`) each have per-scene mood overrides in `NEURAL_SCENE_OVERRIDES`. The `bloomtint`, `density`, and `intensity` values there ALWAYS win over whatever's in your URL builder appearance settings. This is intentional — each scene has a distinct mood (cyan starting / purple BRB / red tech alarm / dim ending) and shouldn't all blend together.

If you want a specific scene to use builder bloomtint instead, remove the `bloomtint` key from that scene's entry in `NEURAL_SCENE_OVERRIDES`.

### Game scene uptime resets on each scene activation

The `#neural-game` overlay shows a live HH:MM:SS uptime counter in the top HUD. With "Refresh browser when scene becomes active" ON in OBS, the timer resets every time you switch INTO the gaming scene. This means it shows "time in current gameplay segment" not "total stream uptime".

If you want true total stream uptime, that requires persisting a start timestamp to localStorage (and refusing to reset on scene activation). Not implemented currently.

### HTML comments + nested SVG comments

When commenting out a large block of HTML that contains inline SVG, watch for nested `<!-- ... -->` inside the SVG. The first inner `-->` will close the outer comment and the rest of your "commented-out" content will render. Use full deletion (or wrap in `<template>`) instead.

This bit us once when removing the legacy AynEnki character view section — the inner `<!-- LEFT WING -->` SVG comments closed the outer wrapping comment.

### URL builder field persistence

Every change to a URL builder field saves the entire builder state to `state.avatarBuilder` in localStorage. Restored on admin entry. The auto-save runs on every `aurlBuild()` call which happens on every form change — there's a guard to bail if the form isn't in the DOM so it can't accidentally wipe state from outside admin.

### Neural scenes inherit appearance from URL builder

When a neural scene initializes its embedded avatar iframe, it calls `neuralScenePreset(kind)` which merges:
1. Your saved URL builder appearance (`avatarAppearanceParams()` — colors, nucleus, halo, veils, morph, theme, gain, scale)
2. The per-scene overrides (density, intensity, bloom tint, sometimes morph)

So changing nucleus color in the URL builder propagates to all five neural scenes automatically.

### The "Mouth doesn't trigger glow" decoupling

Voice level controls bloom strength + brightness. Mouth open controls only the network's scale (subtle breathing). They're explicitly decoupled — opening your mouth without speaking should NOT trigger the network to glow brighter. If it ever does again, check:
- The shader uniforms `uVoiceLevel` and `uMouthOpen` aren't accidentally cross-wired
- The bloom-pass compensation isn't disabled
- There aren't `triggerPulseAt()` calls tied to `mouthOpen` thresholds in animate()

### Body class flags on `<body>`

- `obs-clean` — hides all UI panels (set by `?obs=1`)
- `transparent` — page background goes transparent (set by `?obs=1` or `?transparent=1`)
- `debug` — shows cam preview and diagnostic readout (set by `?debug=1` or pressing `D`)
- `stinger-mode` — total UI lockdown for stinger recording (set by `?stinger=1`)
- `neural-on` — set by master site when on a route that embeds the neural avatar (hides legacy SVG eyes)

### Hotkeys (only when the avatar page has focus)

- `M` — manual pulse
- `F` — freeze the network
- `T` — cycle theme
- `B` — toggle bloom
- `D` — toggle debug overlay
- `H` — hide all panels

---

## postMessage control bridge

When the neural avatar is embedded in an iframe (inside any of the neural scenes or game-scene), the parent page can drive it live via `window.controlAvatar(cmd)`:

```js
controlAvatar({ morph: 6 })                          // switch formation
controlAvatar({ density: 80, theme: 3 })             // change density + theme
controlAvatar({ pulse: true })                       // trigger a pulse
controlAvatar({ paused: true })                      // freeze
controlAvatar({ colors: { inner: 'e8b84b', sparkle: 'a060e0' } })
controlAvatar({ aur: false, eye: true })             // toggle features
controlAvatar({ voiceGain: 1.2, scaleSensitivity: 0.6 })
```

Implemented in `index.html`'s `controlAvatar` function. Posts to the iframe with `type: 'malikeye-control'`. The avatar listens for that message type and routes to action functions.

**Note:** when OBS owns the iframe directly (not through the master site), the postMessage bridge doesn't reach. For OBS-owned Browser Sources, configuration goes through URL params only.

---

## State shape (localStorage key: `malikeye-site-v3`)

Top-level keys on `state`:
- `brand`, `tagline`, `motto`, `siteTitle`, `siteDesc`, `ogImage` — identity
- `socials.{twitch,youtube,twitter,discord}.{url,label}` — social links
- `primaryHandle`, `socEmail`, `socArtstation`, `socKofi`, `socPatreon`, `socBmc` — support links
- `commissionsOpen`, `commissionsStatusOpen`, etc. — commissions config
- `schedule[]` — stream schedule items
- `ss*`, `brb*`, `end*`, `jc*`, `gs*` — Malikeye overlay text
- `fCurrent`, `fGoal`, `sCurrent`, `sGoal` — follower/sub goals
- `ae.{...}` — AynEnki overlay content (brand, handles, stream title, phrases, farewells, timer config)
- `neural.{...}` — neural scene text (handles, brand per scene, phrases for BRB/Tech, countdown minutes)
- `avatarBuilder.{...}` — URL builder field values
- `portfolio[]`, `shop[]` — content items
- `embersEnabled`, `emberSettings.{density,speed,hue1,hue2}` — Malikeye scene particle config
- `password` — admin login

---

## Routing / view system

Single-page app. All views are `<section class="view" id="view-name">` blocks. Router (`route()` function in index.html) shows the active view, hides others, runs the route's init hook from `ROUTE_HOOKS`, and cleans up the previous view.

`OVERLAY_VIEWS` is the list of routes that get fullscreen treatment when reached via hash navigation (no nav bar, no padding, edge-to-edge).

`HIDDEN_VIEWS` includes overlays + admin + quote — they don't appear in the main nav.

---

## Recording for streaming (stinger workflow detail)

Recommended OBS recording settings for the stinger MP4:
- Output → Recording → Format: `mkv` or `mp4`
- Encoder: x264 (or NVENC if available)
- Rate control: CRF
- CRF: 16 (high quality for short clip)
- Keyframe interval: 1
- Profile: high
- Preset: medium

OBS will accept the resulting file as a stinger. If you record as MKV, OBS lets you remux to MP4 from File menu.

For the transition point: set to half your cycle duration. Default cycle is 1000ms so transition point = 500ms. If you used `?stingerdur=0.7` (fast), set transition point = 350ms. If `?stingerdur=1.4` (slow), set transition point = 700ms.

---

## Known TODOs / future polish

- Twitch live status badge (poll Twitch helix-status, light up "● LIVE" on homepage)
- Auto-convert stream schedule to viewer's local timezone (currently ET hardcoded)
- OG image + structured data for link unfurls
- Mobile responsive audit (built mostly for desktop OBS use)
- Lore page with veil glyph decoder (ARG entry point)
- Konami-code easter egg → "Reverie mode"
- True stream uptime timer (persist start time across scene reloads)

---

## Editing safely

- `index.html` is large (~6000+ lines). Use search/find for the section you want before scrolling.
- Major section landmarks: `view-admin` (line ~2900), `NEURAL` blocks (CSS ~2400+, HTML ~2682+, JS ~6500+), `aurlBuild` (~4743), `saveAdmin` (~5984).
- Both files are self-contained — never `import` from each other except via iframe.
- `state` schema is versioned: `malikeye-site-v3` is current. Bumping requires migration logic in `loadState`.

---

Built over many late-night iteration sessions. The brand is the eye that watches; the network is what it sees.
