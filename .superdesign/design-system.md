# XR Resource Finder — Design System

## Product
Internal catalogue for Manchester Metropolitan University (Brooks Building) staff/facilitators to find and launch CAVE experiences, VR headset apps, 360 videos and websites for teaching sessions. Jobs: search fast, filter by type and topic, open an item's details (launch path, objectives, facilitator notes), pin favourites, build a printable Run Sheet for a session. Used on desktop in the CAVE control room and on phones while walking a group through.

## Hard constraints (never change)
- Font: "Plus Jakarta Sans" only (400/500/600/700). Mono for paths: ui-monospace / Cascadia Mono / Consolas.
- Logo: the supplied Manchester Metropolitan University logo, on the brand-blue header strip. Never substitute.
- Brand blue #00AEEF header strip at 100% opacity, black (#111) text/icons on it, in both themes.
- Colour palette — use ONLY these:
  - Light (default): bg #F2F4F6, surface #FFFFFF, surface-2 #EAEEF2, line #D3DBE2, text #16212B, muted #5A6B7B
  - Dark: bg #0B1015, surface #131C25, surface-2 #1B2731, line #27353F, text #E3EBF2, muted #8EA1B3
  - Kind accents (light / dark): CAVE amber #B4700A / #F2A93B; VR teal #0E7C74 / #45D0C4; 360 Video violet #6A44A8 / #B98CF2; Website slate #2F5F94 / #7FA8D4
- No new hues, gradients, or decorative fonts.

## Styling
- Radius: 10px cards/inputs, 8px buttons, pill chips (20px).
- Cards: white surface, 1px line border, 3px left border in the kind accent, 4:1 thumbnail, title 15.5px/620, summary muted 13.5px clamped to 3 lines, tags.
- Shadows: subtle only (0 14px 32px -18px rgba(20,35,50,.4) light).
- Focus ring: 2px teal.
- Breakpoint: 640px phone. Container max 1320px.

## Key UI
Sticky header (blue strip: logo, title + subtitle, search with "/" hint, tools: Surprise me, Calm mode, theme toggle) → Types row (Everything, CAVE, VR, 360 Videos, Websites, Pinned, Run Sheet, New Entry, Help & How-to, each with count) → Categories topic chips (collapsible, collapsed by default on phone) → status line (N results + keyboard hints) → card grid. Item detail = right-side drawer (full-screen sheet on phone).

## Motion
Minimal: 160–200ms ease for drawer/scrim, respect prefers-reduced-motion.
