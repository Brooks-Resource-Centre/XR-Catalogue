# XR Resource Finder

A searchable catalogue of the CAVE experiences, VR headset apps, 360° videos and websites available at the Brooks Building, Manchester Metropolitan University.

**Live site:** https://brooks-resource-centre.github.io/XR-Catalogue/

It works on desktop and phone browsers. Nothing needs installing.

---

## Using the site

| Feature | What it does |
|---|---|
| **Search** | Searches names, descriptions, topics, objectives and launch paths. Press `/` to jump to the search box. |
| **Resource types** | Filter by CAVE, VR, 360 Videos or Websites (left sidebar; a collapsible menu on phones). |
| **Categories** | Tick one or more topics to narrow the results. |
| **Card → details** | Click any card to see the full description, learning objectives, launch path and facilitator notes. |
| **Pin ☆** | Keep favourites in the **Pinned** tab. |
| **Run Sheet** | Add experiences in the order you'll run them for a session, add notes, then print it. |
| **Surprise me / Calm mode / ◐** | Random pick / hide pictures and enlarge text / switch light and dark. |
| **Help & How-to** | Quick fixes for CAVE and headset problems mid-session. |

Pins, the run sheet and display preferences are saved **in your own browser only**. They don't sync between devices or people.

---

## Adding a new entry

New entries are reviewed by the XR team before they appear. The site can't change itself (GitHub Pages only serves files), so submissions go through a GitHub issue, and a GitHub Action adds approved ones to the catalogue.

### 1. Submit it (anyone in the XR team)

1. Sign in to GitHub.
2. On the site, open **New Entry** in the sidebar.
3. Fill in the **name**, **where it runs** and a **one-line summary**. Optionally pick topics, add the launch path or link, and open **More details** for the description, learning objectives, facilitator notes, creator and duration.
4. Press **Send to the XR team**. A pre-filled GitHub issue opens. If your browser blocks the new tab, it opens in the same tab.
5. **Optional picture:** drag an image into the issue's text box, under *Picture (optional)*. Wait for it to finish uploading.
6. Press **Create**.

> Don't edit the `json` block at the bottom of the issue unless you're fixing a typo. If you do, keep its quotes and commas intact.

### 2. Approve it (anyone with write access to this repo)

1. Open the issue under **Issues** and check the details. Edit the issue text to fix anything.
2. In the right-hand sidebar, click **Labels** and tick **`approved`**.
3. Within about a minute, a run called **Add catalogue entry** appears under the **Actions** tab. When it finishes:
   - **Success:** the bot comments *"Added to the catalogue"* and closes the issue. The site updates after GitHub Pages republishes (usually 1–2 minutes). Refresh to see it.
   - **Failure:** the bot comments with a link to the run explaining why. Fix the issue text, then remove and re-add the `approved` label.

To **reject** an entry, close the issue without adding the label.

**Who can approve?** Only people with write access can add labels, and the workflow double-checks this. Anyone with a GitHub account can open an issue on this public repo, but nothing goes live until the XR team approves it.

---

## Removing an entry

Removals go through the same review as new entries.

### 1. Request it (anyone in the XR team)

1. Sign in to GitHub.
2. On the site, open the entry and scroll to the bottom of its details panel.
3. Under *Out of date or no longer available?*, press **Request removal**. A pre-filled GitHub issue titled *Remove entry: …* opens.
4. Say why under *Why?*, then press **Create**. Leave the `json` block as it is, because it holds the entry's id.

### 2. Approve it (anyone with write access to this repo)

1. Open the issue and check that it's the right entry.
2. Add the **`approved`** label.
3. When the run under **Actions** finishes, the bot comments *"Removed from the catalogue"* and closes the issue. The entry disappears from the site after GitHub Pages republishes. If the run fails, the bot comments with a link that explains why.

To **keep** the entry, close the issue without adding the label.

---

## One-time setup (already done if entries are working)

- **Labels:** create `approved`, `new-entry` and `remove-entry` under [Issues → Labels](https://github.com/Brooks-Resource-Centre/XR-Catalogue/labels). The names must match exactly.
- **Workflow permissions:** go to **Settings → Actions → General → Workflow permissions** and choose **Read and write permissions**. If this is greyed out, a Brooks-Resource-Centre organisation admin has to allow it.
- **Branch protection:** if `main` is protected, allow GitHub Actions to push to it, or the bot's commit will be refused.

---

## How it works

The whole site is one static HTML file with no server or database.

| File | Purpose |
|---|---|
| `xr-resource-finder.html` | The app: layout, styles, code **and** an embedded copy of the catalogue data and thumbnail images, so it works offline or as a single file. |
| `xr-catalogue-data.json` | The master catalogue data (items and help guides). |
| `index.html` | Redirects the site's root address to `xr-resource-finder.html`. |
| `img/` | Original thumbnail images. Those already in the catalogue are embedded in the HTML. |
| `merge-mine-entries.ps1` | Adds or removes entries in **both** the JSON file and the HTML's embedded copy, then checks both are valid. |
| `.github/workflows/add-entry.yml` | The GitHub Action that adds or removes an entry when its issue is approved. |
| `XR_Resource_Catalogue.pdf` | The original printed catalogue the data came from. |

**Approving an entry:** the Action reads the `json` block from the issue and keeps only the catalogue's own fields. It shrinks any attached picture to 900px wide and runs `merge-mine-entries.ps1`. Then it commits both files to `main`, and the push makes GitHub Pages republish the site. For a removal request, the Action checks the id exists and runs the same script in removal mode instead.

**Important:** the site reads the data embedded in the HTML, not the JSON file. Editing `xr-catalogue-data.json` by hand **will not** change the site. Always add entries through the issue flow or `merge-mine-entries.ps1`.

### Adding entries manually (bulk or offline)

On a Windows machine with this repo checked out:

```powershell
# entries.json is either {"entries":[ ... ]} or a plain [ ... ] array of entries
.\merge-mine-entries.ps1 -InputFile .\entries.json -DryRun   # preview
.\merge-mine-entries.ps1 -InputFile .\entries.json           # apply
```

Then open `xr-resource-finder.html` locally to check the new cards, and commit and push both files.

An entry looks like this. Only `name` is required; the script fills in the rest of the fields.

```json
{
  "name": "Ambulance Handover",
  "kind": "cave",
  "quick": "Handover scenario filmed in the resus bay",
  "desc": "Longer description…",
  "topics": ["Clinical & Emergency"],
  "path": "C:\\Users\\mmu\\Documents\\IntuiFace\\Ambulance Handover",
  "obj": ["Learning objective one", "Learning objective two"],
  "facilitatorNotes": ["Load the scene before the group arrives"],
  "by": "Creator name",
  "time": "20 minutes"
}
```

`kind` is one of `cave`, `headset` (VR app), `video` (360° video) or `web` (website).

To remove entries by hand, put their ids in a file such as `{"remove": ["some-item-id"]}` and run the same script with it.

### Editing an existing entry

There's no form for this yet. Edit the item in `xr-catalogue-data.json` **and** the matching entry in the `const DATA = {…}` block of `xr-resource-finder.html`, then commit both. Find the item by its `id`. If edits become frequent, a script like the merge script would be worth adding.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| **Send to the XR team** does nothing | Allow pop-ups for the site, or sign in to GitHub and try again. It should fall back to opening GitHub in the same tab. |
| No `approved` label in the issue sidebar | Create it once (see *One-time setup*), or ask someone with write access. |
| No run appears in **Actions** after labelling | Check the workflow file is on `main` and Actions are enabled under **Settings → Actions**. |
| Run fails at **Commit and push** | Workflow permissions aren't *read and write*, or branch protection blocks the bot. |
| Entry added but not on the site | Wait for the **pages-build-deployment** run under Actions to finish, then hard-refresh (`Ctrl+F5`). |
| Removal fails with *"No catalogue entry has the id"* | The entry was already removed, or the `json` block in the issue was edited. Copy the id from a fresh **Request removal** issue. |
| No **Request removal** button | It only appears on entries in the catalogue. Entries saved only in your own browser (old *Mine* entries) have a remove button on their card instead. |
| Picture missing | The upload hadn't finished when the issue was created, or the image couldn't be read. The run log shows "Picture skipped" with the reason. |
