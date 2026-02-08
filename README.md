# Openmusic Landing Page

Single-file marketing page for Open Music Networks. Self-contained HTML
with inline CSS, JavaScript, and a base64-encoded watermark image. No
build step, no dependencies beyond one CDN font.


## File

`index.html` -- the entire page. Drop it on any static host.


## External Dependency

Mona Sans via jsDelivr CDN (`@fontsource/mona-sans@5.0.0`). The page
falls back to system fonts if the CDN is unreachable.


## Page Structure

The page follows a linear narrative arc:

1. **Hero** -- headline, subhead, primary CTA ("Join the beta")
2. **Problem** -- frames the continuity gap in async music collaboration
3. **Solution** -- introduces Openmusic's memory-first approach
4. **Benefits** (x2) -- change context and browser-native access
5. **Positioning Bridge** -- dark-background interstitial contrasting
   real-time tools with between-session continuity
6. **Fit Section** -- who this is for (and who it is not for)
7. **Closure** -- path to release messaging
8. **Final CTA** -- "Start something that lasts" with secondary CTA
9. **Footer** -- logo, nav columns, legal links


## Design Tokens

Colors and spacing are defined as CSS custom properties in `:root`,
following the NeoRelief v2.1 system. Key values:

- `--bg: #F9F9F9` -- page background (true neutral, equal RGB channels)
- `--bg-surface: #ffffff` -- elevated surfaces
- `--accent: #4f46e5` / `--accent-hover: #4338ca` -- indigo CTA color
- Stone scale for text hierarchy (`--text`, `--text-secondary`,
  `--text-muted`, `--text-faint`)

The neutral `#F9F9F9` base and equal-channel overlay
(`rgba(249, 249, 249, 0.93)`) were chosen to minimize color shift
under macOS Night Shift and True Tone display adjustments.


## Watermark

A concrete texture image is embedded as a base64 WebP string inside a
fixed-position `div` at z-index 0. A semi-transparent overlay
(`body::after` at z-index 1) sits on top, allowing ~7% of the texture
to bleed through as subtle background depth.

The watermark image has `filter: grayscale(1)` applied in CSS to strip
any chroma, ensuring only luminance information contributes to the
composited background. This prevents color temperature shifts from
interacting with the texture's color content.


## Mobile Interstitial

Openmusic is a desktop/laptop browser application. On viewports at or
below 768px, tapping any CTA ("Join the beta" in nav, hero CTA, or
"Play on" final CTA) triggers a bottom-sheet modal instead of
navigating to signup.

The interstitial contains:

- A message explaining the desktop requirement
- An email input field for users to send themselves the signup link
- A single-use privacy note ("We'll send one email with the link.
  Nothing else.")

The sheet slides up from the bottom with a backdrop overlay, locks body
scroll, and can be dismissed via backdrop tap, close button, or Escape.

**Backend dependency:** The form submission is currently simulated.
A `POST /api/v1/send-desktop-link` endpoint is needed to send the
actual email via SendGrid. The integration point is marked with `TODO`
in the JavaScript.


## Animations

- **Hero sequence:** staggered `fadeUp` on headline, subhead, and CTA
  with deliberate pacing (0.15s increments)
- **Scroll reveals:** `IntersectionObserver` triggers `.visible` class
  on section headers and content blocks
- **CTA hover:** shimmer pseudo-element sweep, vertical lift, and
  indigo box-shadow
- **Fit list items:** horizontal nudge on hover (disabled on touch)


## Easter Egg

Konami Code (`Up Up Down Down Left Right Left Right B A`) triggers a
chord progression played through the Web Audio API. Triangle-wave
oscillators with ADSR envelopes, nine chords at 120 BPM. Replays
after the progression finishes.


## Responsive Behavior

- **Desktop (> 768px):** full layout, hover effects active, mobile
  interstitial hidden via `display: none`
- **Tablet/Mobile (< 640px):** reduced base font size (16px), tighter
  section gaps, hero reflows to top-aligned, bridge text wraps
  naturally, footer collapses to single column at 480px
- **Small mobile (< 380px):** headline word wrap enabled


## Hosting

No server-side logic required for the page itself. Any static host
works (S3, Netlify, Vercel, GitHub Pages). The mobile interstitial
email feature requires an API endpoint when wired up.


## Editing Notes

- All styles are in a single `<style>` block in `<head>`
- All scripts are in a single `<script>` block before `</body>`
- The watermark image is a long base64 string starting at the
  `<img src="data:image/webp;base64,...">` tag -- do not manually edit
- Section borders between content areas have been intentionally removed
  for a seamless vertical flow
- The `header.scrolled` background uses the same `rgba(249, 249, 249)`
  value as `body::after` to maintain visual continuity when the header
  materializes on scroll
