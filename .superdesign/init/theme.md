# Theme
## Token summary
- Font: "Plus Jakarta Sans" (Google Fonts, 400/500/600/700), fallback Segoe UI/system-ui. Mono: ui-monospace/Cascadia Mono/Consolas.
- Base font 15px / 1.5. Radius --r:10px; chips 20px pill; buttons 8px.
- Brand: --brand-blue #00AEEF (header strip, always 100%, black text on it).
- Light (default): bg #F2F4F6, surf #FFFFFF, surf2 #EAEEF2, line #D3DBE2, fg #16212B, mut #5A6B7B; kind accents amber #B4700A (CAVE), teal #0E7C74 (VR), violet #6A44A8 (360 video), slate #2F5F94 (Website).
- Dark: bg #0B1015, surf #131C25, surf2 #1B2731, line #27353F, fg #E3EBF2, mut #8EA1B3; accents amber #F2A93B, teal #45D0C4, violet #B98CF2, slate #7FA8D4.
- Breakpoint: max-width 640px (phone). Container max-width 1320px.
## Raw tokens
```css
<style>
:root{
  --amber:#F2A93B; --teal:#45D0C4; --violet:#B98CF2; --slate:#7FA8D4;
  --brand-blue:#00AEEF;
  --r:10px;
  --f: "Plus Jakarta Sans","Segoe UI Variable Text","Segoe UI",system-ui,-apple-system,"Helvetica Neue",sans-serif;
  --m: ui-monospace,"Cascadia Mono","SF Mono",Consolas,monospace;
}
html[data-theme="dark"]{
  --bg:#0B1015; --surf:#131C25; --surf2:#1B2731; --line:#27353F;
  --fg:#E3EBF2; --mut:#8EA1B3; --shadow:0 18px 40px -18px rgba(0,0,0,.85);
}
html[data-theme="light"]{
  --bg:#F2F4F6; --surf:#FFFFFF; --surf2:#EAEEF2; --line:#D3DBE2;
  --fg:#16212B; --mut:#5A6B7B; --shadow:0 14px 32px -18px rgba(20,35,50,.4);
  --amber:#B4700A; --teal:#0E7C74; --violet:#6A44A8; --slate:#2F5F94;
}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--fg);font-family:var(--f);font-size:15px;line-height:1.5;
  -webkit-font-smoothing:antialiased}
button,input,textarea,select{font:inherit;color:inherit}
button{cursor:pointer;background:none;border:none}
:focus-visible{outline:2px solid var(--teal);outline-offset:2px;border-radius:4px}
```
