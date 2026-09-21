# TSC AutoCombat

A combat bot for **The Second Calling**, where every character is three classes
at once. It runs all three for you: casts your spells, fires your AAs and
discs, pulls, controls your pets, keeps your boxes together and sits to med
when it is safe. Everything is set up in one in-game window — no macros to
write.

This repository is a **complete MacroQuest** with the tool already in it.
Download it, run `MacroQuest.exe`, and log in.

# ⬇ [Download the latest release](https://github.com/Aoezpz/TSC-AutoCombat/releases/latest)

The zip is about 56 MB — grab it from the **Assets** list on that page.
([every release](https://github.com/Aoezpz/TSC-AutoCombat/releases))

> **Test build.** It has not had much time in game yet. Please report anything
> that looks wrong rather than assuming it is meant to work that way.

---

## Install

1. **Download the zip above** and extract it somewhere like `C:\TSC-AutoCombat`.
   (Or `git clone` this repository — same files.)
2. **Add that folder to your antivirus exclusions.** Every MacroQuest build
   trips Defender and SmartScreen; this one is no different.
3. **Get the zone navmeshes** — they are not in the download (2 GB, and they
   do not compress). Either copy `resources\MQ2Nav` from a MacroQuest install
   you already have, or run `MeshUpdater.exe` from this folder. Without them
   the bot still fights, but it cannot path: no pulling, no chasing, no
   return-to-camp.
4. **Run `MacroQuest.exe`** from this folder, then log in.

The tool opens by itself when you enter the world. If you close it, type `/ac`.

---

## First run

1. **Check your classes.** The header shows the three it detected, each in its
   house colour — melee red, priest silver, caster blue. Wrong? Re-detect under
   **Settings → General**.
2. **Set up what to cast.** Work through the tabs: **Spell Gems**, **Abilities**,
   **AAs**, **Discs**, **Clickies**. Each entry gets a plain rule for when to
   fire — *Target HP below 90%*, *My HP below 40%*, *Missing buff*, *Always*.
   **Import Bar** on the Spell Gems tab fills the list from whatever you have
   memorised.
3. **Pick a mode** on the **Control** tab and press **START**.

Your setup saves itself to `config\` and reloads next time. One file per
character, so boxes never overwrite each other.

---

## Modes

| Mode | Use it when | What it does |
|---|---|---|
| **Manual** | You want to drive | You move and pick targets; it fights, heals and casts. |
| **Puller · Camp** | You are the puller | Runs out, tags a mob, brings it back to camp, kills it there. |
| **Puller · Hunt** | Solo roaming | Wanders and kills mobs where they stand, inside an anchor circle. |
| **Assist · Chase** | Boxed melee | Follows the main assist and attacks their target. |
| **Assist · Camp** | Boxed, holding a spot | Stays at camp, fights what comes in. |
| **Assist · Backline** | Boxed healers and casters | Holds at range, never walks into melee. |

Set the main assist with `/ac ma <name>` or on the Control tab. **Combat style**
(`/ac style melee|ranged|spell`) decides whether you close to melee, shoot a
bow, or stand off and only cast.

---

## Commands

`/ac help` prints the full list in game.

| Command | What it does |
|---|---|
| `/ac` | Start or pause |
| `/ac run` / `/ac pause` | Start / pause |
| `/ac manual`, `/ac puller camp`, `/ac puller hunt`, `/ac assist chase`, `/ac assist camp`, `/ac backline` | Switch mode |
| `/ac ma <name>` | Set the main assist |
| `/ac burn` | Toggle burn mode — fires everything marked *Burn Only* |
| `/ac memall` | Memorise any missing spells |
| `/ac importbar` | Build the spell list from your memorised gems |
| `/ac style melee\|ranged\|spell` | Set combat style |
| `/ac wp add` / `/ac wp clear` | Add a patrol waypoint here / clear the route |
| `/ac net <all\|zone\|group\|Name> <command>` | Run a command on your other boxes |
| `/ac compact` | Shrink to the mini strip |
| `/ac scale 1.25` | Make every window bigger or smaller |
| `/ac status` | Print what the bot is doing |
| `/tscacrun` | Start / pause — bind this to a key |

`/tscac` works anywhere `/ac` does.

---

## Box Network

Your characters on one PC talk to each other over MacroQuest Actors — nothing
extra to install. Assist boxes follow the main assist's real target, two
pullers stay off each other's mobs, boxes count as allies for buff requests,
and `/ac net all burn on` runs a command on every one of them. Settings live
under **Settings → Box Network**.

---

## When something goes wrong

- **Not moving or pulling?** Check the **Status** tab. It shows whether MQ2Nav
  and MQ2MoveUtils are loaded and whether the zone has a navmesh, with buttons
  to load or reload them. No navmesh is the usual answer — see step 3 above.
- **Stuck on terrain?** It remembers where it got stuck and routes around it
  next time. Clear those spots under **Settings → Navigation**.
- **Reporting a bug:** turn on **Log To File** (`/ac log on`), and when it
  happens run `/ac dump`. Attach the files from the `Logs\` folder. `/ac debug`
  prints extra detail to chat.
- **Want to see every ability as it fires?** Turn on **Debug Mode**
  (`/ac debug`). Normally each AA, disc, skill and clickie is recorded quietly
  in the log — `/ac dump` still shows exactly what went off — rather than
  announced in chat, because at a few lines per swing it buried everything else.
- **Crash on startup?** Do not swap in a MacroQuest you downloaded elsewhere.
  This client is patched, and a stock MacroQuest build crashes the moment it
  loads. Use the one in this folder.
- **That small "MQ" window with the squashed tabs?** That is MacroQuest's own
  chat window (MQ2ChatWnd), not this tool. It is 400×200 in the top-left corner
  the first time you play a server, and it borrows the game's chat-window
  template — which is why your EQ chat tabs are drawn across it and overlap at
  that width. Fixes, in order of effort:
  - **Drag it wider.** The size and position save per character, so you only do
    it once. `/mqchat reset` brings it back if it ends up off-screen.
  - `/mqchat SaveByChar off` — one size and position for every character, handy
    when boxing.
  - **Turn it off entirely:** `/plugin mq2chatwnd unload`, or set
    `mq2chatwnd=0` under `[Plugins]` in `config\MacroQuest.ini`. MacroQuest's
    output then goes to your normal EQ chat windows instead.

  The bot itself no longer writes to it every swing — see below.

---

## Where your files live

| File | What it holds |
|---|---|
| `config\tscac_loadout_<server>_<char>.lua` | Each character's settings. Never overwritten by an update. |
| `config\tscac_data.lua` | Spell, disc and AA lists for all 16 classes. |
| `Logs\tscac_*.log` | Diagnostic logs, when Log To File is on. |
| `lua\tscac.lua` | The engine. |
| `lua\tscac\` | Box Network and the logger. |
| `resources\MQ2Nav\` | Zone navmeshes — you supply these. |

To update: pull (or download the ZIP again) and overwrite. Your `config\`
loadouts are ignored by git and will not be touched.

---

## Credits

Built on the engine of an open-source MacroQuest combat bot, stripped down to
the bot itself and rebuilt for this server: its own theme, the trio's house
colours, and none of the companion windows (chat, map, DPS meter, inventory
and the rest) that belong to other tools.

MacroQuest and its plugins are third-party software — see `resources\LICENSE.md`
and `resources\CHANGELOG.md`.
