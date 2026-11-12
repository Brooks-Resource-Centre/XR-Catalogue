# Components
Vanilla HTML/CSS/JS single file `xr-resource-finder.html` (no framework, no Tailwind). Components are template-string functions in the inline <script>.

## Card — `cardHTML(i, idx)` (resource card in results grid)
```js
function cardHTML(i, idx){
  const k = KIND[i.kind] || KIND.mine;
  const pinned = state.pins.includes(i.id);
  const img = IMG[i.img] || i.imgData;
  const mine = state.mine.some(m => m.id === i.id);
  return `<div class="card${idx===state.sel?' sel':''}" style="--dot:${k.colour}" data-id="${i.id}" tabindex="0" role="button">
    ${img ? `<img class="thumb" src="${img}" alt="" loading="lazy">`
          : `<div class="noimg">${k.label}</div>`}
    <button class="pin" data-pin="${i.id}" aria-pressed="${pinned}" title="${pinned?'Unpin':'Pin'}">${pinned?'★':'☆'}</button>
    ${mine ? `<button class="pin" style="right:40px" data-rmmine="${i.id}" title="Remove this entry">×</button>` : ''}
    <div class="cbody">
      <div class="ctitle">${esc(i.name)}</div>
      <div class="cquick">${esc(i.quick)}</div>
      <div class="cmeta">
        <span class="tag k">${k.label}</span>
        ${(i.topics||[]).slice(0,2).filter(t=>t!=='Other').map(t=>`<span class="tag">${esc(t)}</span>`).join('')}
        ${i.time ? `<span class="tag">${esc(i.time)}</span>` : ''}
      </div>
    </div></div>`;
}
```
## Chips / tabs / buttons — CSS
```css
/* ---------- channels ---------- */
.chan{max-width:1320px;margin:0 auto;padding:12px 20px 0;display:flex;flex-wrap:wrap;row-gap:4px;gap:4px;overflow-x:auto;scrollbar-width:none}
.chan::-webkit-scrollbar{display:none}
.ch{padding:9px 14px 11px;border-bottom:2.5px solid transparent;color:var(--mut);font-size:12.5px;
  white-space:nowrap;display:flex;gap:7px;align-items:center;font-weight:500}
.ch:hover{color:var(--fg)}
.ch[aria-selected="true"]{color:var(--fg);border-bottom-color:var(--dot,var(--fg))}
.ch .n{font-size:12.5px;background:var(--surf2);border-radius:20px;padding:1px 7px;color:var(--mut)}
.ch[aria-selected="true"] .n{background:var(--dot,var(--fg));color:var(--bg)}

/* ---------- topic chips ---------- */
.topics{max-width:1320px;margin:0 auto;padding:12px 20px 14px;display:flex;gap:7px;flex-wrap:wrap;align-items:center}
.chip{border:1px solid var(--line);background:var(--surf);border-radius:20px;padding:5px 12px;font-size:12.5px;color:var(--mut)}
.chip:hover{color:var(--fg);border-color:var(--mut)}
.chip[aria-pressed="true"]{background:var(--fg);color:var(--bg);border-color:var(--fg);font-weight:500}
.chip.reset{border-style:dashed;color:var(--amber);border-color:var(--amber)}
.topics-toggle{cursor:pointer;font-weight:500;display:flex;align-items:center;gap:6px}
.topics-toggle[aria-expanded="true"]{color:var(--fg);border-color:var(--mut)}
.topics-list{display:contents}
.topics-list.hidden{display:none}
.sec{border-top:1px solid var(--line);padding:16px 0 4px}
.sec h3{font-size:12.5px;color:var(--mut);margin:0 0 9px;font-weight:600;letter-spacing:.01em}
.sec p{margin:0 0 10px;max-width:64ch;line-height:1.58}
.sec ul{margin:0;padding-left:18px;max-width:64ch}
.sec li{margin-bottom:7px;line-height:1.5}
.facts{display:grid;grid-template-columns:auto 1fr;gap:7px 16px;font-size:13.5px}
.facts dt{color:var(--mut)}
.facts dd{margin:0}
```
