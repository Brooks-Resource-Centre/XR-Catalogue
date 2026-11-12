# Layouts
## Header shell (`xr-resource-finder.html` lines 238-261) + drawer (266-278)
Sticky header: brand-blue strip (logo + title + search + tools), then Types tab row (#chan), then Categories chips (#topics). Main = status line + grid. Right-side drawer for item detail.
```html
<header class="bar">
  <div class="bar-in">
   <div class="bar-in-i">
    <div class="brand">
      <img class="logo" src="(MMU logo PNG)" alt="Manchester Metropolitan University">
      <b>XR Resource Finder<span>CAVE &amp; XR Resources · Brooks Building</span></b>
    </div>
    <div class="searchwrap">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/></svg>
      <input id="q" type="search" placeholder="Search everything — try &ldquo;dementia&rdquo;, &ldquo;flood&rdquo;, &ldquo;ward&rdquo;" autocomplete="off" spellcheck="false">
      <span class="kbd" id="kbdhint">/</span>
      <button id="clearq" title="Clear search" aria-label="Clear search">&times;</button>
    </div>
    <div class="tools">
      <button class="tool" id="lucky" title="Show me a random one">Surprise me</button>
      <button class="tool" id="calm" aria-pressed="false" title="Hide pictures, one column, bigger text">Calm mode</button>
      <button class="tool" id="theme" title="Switch light / dark">◐</button>
    </div>
   </div>
  </div>
  <nav class="chan" id="chan"></nav>
  <div class="topics" id="topics"></div>
</header>

<main>
  <div class="status" id="status"></div>
  <div id="view"></div>
</main>

<div class="scrim" id="scrim"></div>
<aside class="drawer" id="drawer" role="dialog" aria-modal="true" aria-labelledby="dtitle" tabindex="-1">
  <div class="dhead">
    <span class="lbl" id="dkind"></span>
    <div style="display:flex;gap:6px">
      <button class="mini" id="dpin" title="Pin this" aria-pressed="false">☆</button>
      <button class="mini" id="dclose" title="Close (Esc)">&times;</button>
    </div>
  </div>
  <div class="dbody" id="dbody"></div>
</aside>
<div class="toast" id="toast"></div>

```
```js
function drawTabs(){
  const tabs = [
    ['all','Everything','var(--fg)',count('all')],
    ['cave','CAVE','var(--amber)',count('cave')],
    ['headset','VR','var(--teal)',count('headset')],
    ['video','360 Videos','var(--violet)',count('video')],
    ['web','Websites','var(--slate)',count('web')],
    ['pinned','Pinned','var(--amber)',state.pins.length],
    ['run','Run Sheet','var(--teal)',state.run.length],
    ['mine','New Entry','var(--slate)',state.mine.length],
    ['help','Help &amp; How-to','var(--slate)',null]
  ];
  const cur = tabs.find(t => t[0] === state.tab);
  document.getElementById('chan').innerHTML = `
    <button class="chip topics-toggle" id="typesToggle" aria-expanded="${state.typesOpen}" aria-controls="typesList" title="Show or hide types">
      <span aria-hidden="true">☰</span> Types${cur ? ` <span style="opacity:.6">(${cur[1]})</span>` : ''}
    </button>
    <div class="topics-list${state.typesOpen ? '' : ' hidden'}" id="typesList" role="tablist">${tabs.map(([k,l,c,n]) =>
    `<button class="ch" role="tab" data-tab="${k}" style="--dot:${c}" aria-selected="${state.tab===k}">
       ${l}${n!==null?`<span class="n">${n}</span>`:''}</button>`).join('')}</div>`;
}
function drawTopics(){
  const el = document.getElementById('topics');
  if (['run','help','mine'].includes(state.tab)){ el.innerHTML=''; return; }
  const c = {};
  pool().forEach(i => (i.topics||[]).forEach(t => c[t] = (c[t]||0)+1));
  const list = Object.entries(c).sort((a,b) => b[1]-a[1]).filter(([t]) => t !== 'Other');
  const chips =
    list.map(([t,n]) => `<button class="chip" data-topic="${esc(t)}" aria-pressed="${state.topics.has(t)}">${esc(t)} <span style="opacity:.6">${n}</span></button>`).join('')
    + (state.topics.size || state.q ? `<button class="chip reset" id="resetall">Clear filters</button>` : '');
  el.innerHTML = `
    <button class="chip topics-toggle" id="topicsToggle" aria-expanded="${state.topicsOpen}" aria-controls="topicsList" title="Show or hide categories">
      <span aria-hidden="true">☰</span> Categories${state.topics.size ? ` <span style="opacity:.6">(${state.topics.size})</span>` : ''}
    </button>
    <div class="topics-list${state.topicsOpen ? '' : ' hidden'}" id="topicsList">${chips}</div>`;
}
```
