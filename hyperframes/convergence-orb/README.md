# Convergence — Liquid Glass Orb

A 12-second cinematic motion-graphics sequence built with
[HeyGen HyperFrames](https://github.com/heygen-com/hyperframes) — the open-source
"write HTML → render video" framework. The scene is a Three.js (WebGL) composition
rendered deterministically to MP4.

## The sequence

1. **Approach (0–5s)** — two thick, coiling metallic ribbons drift toward each
   other in a pitch-black studio. One glows sapphire blue, the other molten copper.
   The camera drifts forward and orbits with a shallow depth-of-field / motion feel.
2. **Collision (≈5–7s)** — the ribbons rush to centre, collapse, and merge in a
   burst of light.
3. **Orb hero (7–12s)** — the collision forms a reflective glass orb: a black
   dielectric shell catching sharp studio softbox highlights, a blue↔copper
   chromatic Fresnel rim, and a turbulent trapped-energy core swirling with
   blue/copper plasma. The camera slowly orbits and pushes in.

## How it's made

- **Rendering**: Three.js `r147` scene rendered by HyperFrames via headless
  Chromium + FFmpeg. All animation is driven by a single paused GSAP timeline
  (`window.__timelines.main`) that HyperFrames seeks frame-by-frame — no
  autoplay, no `Date.now()`, no unseeded randomness, so every frame is
  deterministic and seek-safe.
- **Look**: PMREM environment map (procedural softboxes) for reflections,
  `UnrealBloomPass` for glow, and a custom post shader for fake shallow-DoF radial
  blur, chromatic aberration, cinematic grade, vignette, and film grain.
- **Vendored libraries**: `three.min.js`, `gsap.min.js`, and the six
  three.js example post-processing scripts live under `assets/vendor/` and load
  via relative paths, so the composition renders fully offline with no CDN.

> Note: HyperFrames renders *code*, not generative video. This is a stylized
> code-built interpretation of the brief (glass/chrome ribbons merging into a
> glass orb) — real WebGL 3D, not a photoreal ray-traced/octane render.

## Re-rendering

Prerequisites: Node.js 22+, FFmpeg, and a Chromium that HyperFrames can drive.

```bash
cd hyperframes/convergence-orb

# Preview live in the browser
npx hyperframes preview

# Validate the composition
npx hyperframes lint
npx hyperframes check

# Render to MP4 (force software GL if no GPU is available)
npx hyperframes render --no-browser-gpu --fps 30 -o renders/convergence.mp4
```

Output: 1920×1080, 30fps, 12s, H.264 MP4. Render outputs land in `renders/`
(gitignored).
