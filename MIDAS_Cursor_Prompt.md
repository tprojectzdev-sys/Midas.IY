# MIDAS — Full Rebrand & Customization Prompt for Cursor

## 📁 File to Send Cursor

**Send: `midas.lua`** — the already-rebranded file (NOT the original discontinued one, NOT the raw infiniteyield source). This is the file Claude already rebranded with the name MIDAS and version v6.4.2.

---

## 🧠 Context for Cursor

This is a Roblox exploit script (Lua) originally called "Infinite Yield FE" that has been partially rebranded to "MIDAS v6.4.2". It is 13,341 lines long. The script is a full admin/command GUI with hundreds of commands, a settings system, keybinds, waypoints, aliases, plugins, an event editor, ESP, chat logs, and more.

**The rebrand is ~60% complete. Your job is to finish it cleanly without breaking any functionality.**

---

## ✅ SECTION 1 — Already Done (DO NOT re-do or undo these)

- `currentVersion` changed to `"6.4.2"` ✅
- Title bar shows `"MIDAS v6.4.2"` ✅
- Most visible text saying "Infinite Yield" → "MIDAS" ✅
- Internal Lua globals `IY_LOADED`, `IY_DEBUG`, `IYMouse` were intentionally left untouched ✅
- Save file `IY_FE.iy` filename was intentionally left untouched ✅
- Command names `hideiy`, `showiy`, `keepiy`, `unkeepiy`, `togglekeepiy` were intentionally left untouched ✅
- GitHub asset URLs (`infyiff/backup`, `EdgeIY/infiniteyield`) were intentionally left untouched ✅

---

## 🔧 SECTION 2 — Tasks for Cursor to Complete

### 2A. Discord Link — Replace ALL instances of `discord.gg/78ZuWSq` and `discord.com/invite/78ZuWSq`

There are **5 locations** to update:


| Line | What                                                                          | Replace with                   |
| ---- | ----------------------------------------------------------------------------- | ------------------------------ |
| 1607 | `PluginsHint.Text` string                                                     | `discord.gg/[YOUR_CODE]`       |
| 2884 | Inline table `Text=` for invite button label                                  | `discord.gg/[YOUR_CODE]`       |
| 2902 | `toClipboard("https://discord.gg/78ZuWSq")`                                   | your full invite URL           |
| 2911 | `inviteButton.Text = "Copy Discord Invite Link (https://discord.gg/78ZuWSq)"` | your code                      |
| 6533 | `toClipboard('https://discord.com/invite/78ZuWSq')`                           | your full invite URL           |
| 6534 | `notify('Discord Invite', 'Copied to clipboard!\ndiscord.gg/78ZuWSq')`        | your code                      |
| 6536 | `notify('Discord Invite', 'discord.gg/78ZuWSq')`                              | your code                      |
| 6549 | `args = {code = '78ZuWSq'}`                                                   | your invite code only (no URL) |


**⚠️ IMPORTANT:** Only change the invite code/URL. Do not change surrounding logic, variable names, or the Discord RPC join flow on lines 6540–6549.

---

### 2B. Credits Text — Line 770

```lua
Credits.Text = "Edge // Zwolf // Moon // Toon // Peyton // ATP"
```

Replace with your own credits. This shows briefly on the intro splash screen before fading out.

**Question for you:** What names/handles do you want credited? (e.g. your username, team name, etc.)

---

### 2C. Logo Image — Line 758

```lua
Logo.Image = getcustomasset("infiniteyield/assets/logo.png")
```

This is the image shown on the intro splash. It currently uses the original IY logo (rbxassetid://1352543873).

**Options:**

- Replace with your own Roblox asset ID: change the rbxassetid in the `iyassets` table at line 116
- Or point it to a custom URL hosted on your GitHub

**Question for you:** Do you have a MIDAS logo image? If so, what's the Roblox asset ID or URL?

---

### 2D. GitHub Backup Repo — Lines 137, 6913, 8751, 8817, 10425, 10447, 10534, 10539

These all pull external Lua scripts from `https://raw.githubusercontent.com/infyiff/backup/...`:

- `serverscanner.lua`
- `f3x.lua`
- `wallwalker.lua`
- `console.lua`
- `dex.lua`
- `SimpleSpyV3/main.lua`
- `audiologger.lua`
- Also: the asset image backup folder at line 137

**Question for you:** Do you have your own GitHub repo to host these from? If yes, provide the repo URL (e.g. `https://raw.githubusercontent.com/YOURUSERNAME/YOURREPO/main/`). If no, you have two choices:

- A) Keep using the original infyiff URLs (they still work, just not "yours")
- B) Fork the infyiff/backup repo and point to your fork

---

### 2E. Version Check & Update Notification — Lines 13231–13237

```lua
local versionJson = game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/version")
```

This checks the original IY version file. If the version doesn't match, it shows an "Outdated" notification.

**Options:**

- Point to your own GitHub version file (a simple JSON file: `{"Version": "6.4.2", "Announcement": ""}`)
- Or disable the version check entirely (wrap it in `if false then ... end`)

**Question for you:** Do you want a live version checker? If yes, what GitHub repo will host your `version` file?

---

### 2F. KeepIY Teleport Script — Line 6486

```lua
queueteleport("loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()")
```

This is the script that auto-runs after a teleport. It currently re-loads the original IY source.

**Replace with:** the raw URL to YOUR hosted `midas.lua` file.

**Question for you:** Where will you host the final `midas.lua`? (GitHub raw URL, pastebin, etc.)

---

### 2G. "Outdated" notification text — Line 13237

```lua
notify("Outdated", "Get the new version at MIDAS")
```

This was already partially rebranded but the message is vague. Replace with a real URL once you have one, e.g.:

```lua
notify("Outdated", "Get the new version at raw.githubusercontent.com/YOU/midas/main/midas.lua")
```

---

### 2H. Asset Folder Name — Lines 138, 139, 145, 148 and all `getcustomasset()` calls

```lua
for _, folder in {"infiniteyield", "infiniteyield/assets"} do
```

All local asset files are stored in a folder called `infiniteyield/` on the user's exploit filesystem. This is safe to rename to `midas/` and `midas/assets/` — BUT only if you also update every single `getcustomasset("infiniteyield/assets/...")` call and every key in the `iyassets` table.

**⚠️ CURSOR MUST do a careful find-and-replace of ALL occurrences:**

- In the `iyassets` table keys (lines 109–121)
- In every `getcustomasset("infiniteyield/assets/...")` call throughout the file
- In the folder creation loop (lines 138–148)
- In the `path:gsub("infiniteyield/", assets)` substitution on line 145

This is safe and will make the local cache use a `midas/` folder instead.

---

### 2I. Save File Name — `IY_FE.iy`

The settings save file is currently named `IY_FE.iy`. This is used in multiple places (lines 3055, 3085, 3090, 3092, 3102, 3104, 3150, 6289, 6294, 6298, 6299, 6317, 6318).

**This is safe to rename to `MIDAS.iy`**, but Cursor must replace ALL occurrences consistently. If even one is missed, saves will break.

**⚠️ Special case on line 6289:**

```lua
if name:lower() == 'plugin file name' or name:lower() == 'iy_fe.iy' or name == 'iy_fe' then
```

Both `'iy_fe.iy'` and `'iy_fe'` must be updated to `'midas.iy'` and `'midas'`.

---

### 2J. Command Names (Optional — Risky)

The commands `hideiy`, `showiy`, `unhideiy`, `keepiy`, `unkeepiy`, `togglekeepiy` still use "iy" in their names. These are what users type in the command bar.

**If you want to rename them to `hidemidas`, `showmidas`, etc.:**
Cursor must update both:

- The `CMDs` table entries (lines 4494–4498) — the `NAME` field
- The `addcmd(...)` calls (lines 6555, 6565, 6575, 7782, 7794) — the first argument AND the alias table

**⚠️ Risk:** If any users or plugins rely on these command names, they'll break. Recommend keeping old names as aliases:

```lua
addcmd('hidemidas', {'hideiy'}, function(...) ... end)
```

---

## ❓ SECTION 3 — Questions You Must Answer Before Cursor Starts

Answer these and paste the answers into the Cursor chat alongside this prompt and `midas.lua`:

```
Q1: What is your Discord server invite code?
    (just the code, e.g. if discord.gg/abc123 then answer: abc123)
    A: https://discord.gg/CrNCnc7pYT

Q2: What names do you want in the credits splash screen?
    (shown briefly on load, e.g. "YourName // Handle2")
    A: i want it to say TQk.z it should show brifley on load 

Q3: Do you have a Roblox asset ID for a MIDAS logo image?
    (rbxassetid://XXXXXXXXXX — upload an image to Roblox to get one)
    A: 75972699875615

Q4: Do you have a GitHub repo to host midas.lua and the backup tools?
    (e.g. github.com/yourname/midas)
    A:"Leave a placeholder: https://raw.githubusercontent.com/YOURUSERNAME/midas/main/midas.lua — I'll fill in the real URL after uploading"

Q5: Do you want the version checker enabled?
    If yes, where will your version JSON file be hosted?
    A: Use placeholder https://raw.githubusercontent.com/YOURUSERNAME/midas/main/version — I'll replace YOURUSERNAME after uploading"

Q6: Do you want to rename the local asset folder from infiniteyield/ to midas/?
    A: yes / no yes 

Q7: Do you want to rename the save file from IY_FE.iy to MIDAS.iy?
    A: yes / noyes 

Q8: Do you want to rename the typed commands (hideiy → hidemidas, etc.)?
    A: no because it might create a heacake with other commands like u are sure it will no break anything then yes rename 

Q9: Do you want to rename the internal globals IY_LOADED, IY_DEBUG, IYMidas?
    (Advanced — only do this if you know what you're doing, risk of breaking things)
    A: no
```

---

## 📋 SECTION 4 — Instructions for Cursor

```
You are editing a 13,341-line Roblox Lua script called midas.lua.
This is a rebranded version of "Infinite Yield FE" now called "MIDAS v6.4.2".

RULES:
1. Do NOT change any logic, conditions, or functionality — only strings and names as directed.
2. Do NOT rename Lua variables IY_LOADED, IY_DEBUG, or IYMouse unless explicitly told to.
3. When doing find-and-replace, be surgical — check context before replacing.
4. If a replacement could break something (e.g. a URL that 404s), flag it and ask.
5. After all changes, search for any remaining "Infinite Yield", "infiniteyield" (not in asset paths unless task 2H is requested), "infyiff" (not in GitHub URLs unless task 2D is requested), "IY" (standalone), and "78ZuWSq" and report them.
6. Do not reformat, re-indent, or touch any code you weren't asked to change.
7. Work through each section (2A through 2J) one at a time and confirm before moving to the next.
```

---

## 🗂️ Summary of What File to Give Cursor


| File                                        | Use?                  |
| ------------------------------------------- | --------------------- |
| `midas.lua` (the rebranded one Claude made) | ✅ YES — send this one |
| Original `_lua` (the new IY source)         | ❌ No                  |
| Old discontinued script                     | ❌ No                  |


---

*Generated by Claude for MIDAS rebrand project — answer the Q3 questions above, paste them + this doc into Cursor alongside midas.lua, and let Cursor handle the rest.*