# Mon-Stat-Solver
A tool to help calculate real in game numbers and help balance monster toughness in Diablo II Resurrected

Sep 24, 2026 · @KainAbel666 created with Claude

## What the tool does

You tell the Monster Stat Solver how tough you want a monster to be in game, and it tells you what to type into monstats.txt to get exactly that.

Diablo II Resurrected doesn't use monstats.txt numbers directly. Each value is scaled by monlvl.txt according to the monster's level, so a Zombie's Min HP of 101 becomes 7 HP at level 1 in Normal but 4317 HP at level 67 in Hell. Working backwards by hand is slow and easy to get wrong. The solver does that math for you, for all three difficulties at once.

It is built for modders. It runs entirely in your web browser from a single page, and the files you load are read on your own computer. Nothing is uploaded anywhere.

## Quick start

You can make your first change in about five minutes with just your mod's monstats.txt.

1. Open the solver page. A copy of monlvl.txt is built in, so you can skip that file for now.
2. On the **monstats.txt** card, click **Choose file** (or drag the file onto the card) and pick your mod's monstats.txt.
3. Click the **Monster** box and start typing, for example `zombie`. Pick **zombie1 (Zombie)** from the list.
4. The grid fills with the monster's current values. In the **Hell** column, change the **Min HP** target to `6000` and **Max HP** to `8000`.
5. Look at the **Input** column next to each target. That is the number monstats.txt needs. Gold means it will change.
6. Click **Apply** (it shows how many changes are waiting), then **Download monstats.txt**.
7. Copy the downloaded file into your mod's `data/global/excel` folder, replacing the old one.

That's the whole loop: pick a monster, type the in-game values you want, apply, download. Everything below explains the extra tools that make those numbers accurate.

## The data files

Only monstats.txt is needed to edit monsters; every other file makes the numbers more accurate or the lists easier to read. Load each one on its card at the top of the page, either with **Choose file** or by dragging the file onto the card.

| File | Needed? | Where to find it | What it adds |
| --- | --- | --- | --- |
| monstats.txt | Yes | `data/global/excel` | The monsters you edit. The download is this file with your changes. |
| monlvl.txt | Built in | `data/global/excel` | The level scaling tables. Load your mod's copy if it changes them. |
| levels.txt | Optional | `data/global/excel` | Areas, their monster levels and which monsters spawn where. |
| levels.json | Optional | `data/local/lng/strings` | English area names instead of internal keys. |
| monsters.json | Optional | `data/local/lng/strings` | English monster names instead of internal keys. |
| elemtypes.txt | Optional | `data/global/excel` | Your mod's list of element types. A standard list is built in. |
| monai.txt | Optional | `data/global/excel` | Every AI type, plus what each AI parameter does. |
| MonProp.txt | Optional | `data/global/excel` | The extra properties each monster gets, such as resistances or auras. |
| properties.txt | Optional | `data/global/excel` | Turns property codes into the stats they set. |
| itemstatcost.txt | Optional | `data/global/excel` | How each stat is described in game. |
| skills.txt | Optional | `data/global/excel` | Skill names for chance-to-cast, aura and +skill properties. |
| item-modifiers.json | Optional | `data/local/lng/strings` | The English text for property descriptions. |

The last five files sit in their own **Monster properties** row of cards and work together to describe a monster's MonProp.txt properties.

Each card shows what it loaded, such as "751 monsters" or "4 of 6 areas matched". If a file is the wrong one or is missing a column, the card says so in red and names what's missing.

The page remembers your files in this browser, so they are still there next time you open it. Your applied edits are remembered too. Use **Forget files** at the bottom to clear everything.

## Picking a monster and its area

A monster's stats depend on its level, and in Nightmare and Hell that level comes from the area it spawns in. The **Pick by** switch gives you two ways to choose both.

### Monster, then area

Type in the **Monster** box to search by Id, English name or name key. Each result shows the levels that monster will have, as Normal / Nightmare / Hell. A small "4 areas" note means it spawns in several places, and the levels show as a range.

Then use the **Area** box to pick where it spawns. Areas where the monster appears are listed first under "Where zombie1 spawns".

The page picks the area for you when it can:

- If the monster spawns in only one area, that area is chosen automatically.
- If it spawns in several, pick one yourself. The status line at the bottom tells you which.
- Bosses always keep their own levels, whatever area is chosen.

### Area's monsters

This mode puts everything in one list. Areas appear in levels.txt order, each with a gold header showing its levels. Under each header are the monsters from that area's spawn list, in order, with each monster's minions indented beneath it.

- Type an area name to see all its monsters, or a monster name to see every area it's in.
- A monster can only be picked where it first appears. Later appearances are greyed out and name the first area, for example "Zombie (Blood Moor)".
- To use a monster with a different area, switch back to **Monster, then area** and pick the area there.

### Normal uses area level

In the unmodified game, Normal monsters use their own monstats.txt level and only Nightmare and Hell use the area's level. Tick **Normal uses area level (patched game)** if your mod changes that, so Normal uses the area level too.

### Manual entry

Pick **Manual entry** at the top of the monster list to use the page as a calculator. Type any level and target values to see what monstats.txt would need. Nothing is written to your file in this mode, and you can give the entry a name in the **Monster name** box.

## Reading the grid

The grid has one group of columns for each difficulty. You type into **Target**; the other three columns are worked out for you.

| Column | What it shows |
| --- | --- |
| Target | The value you want the monster to have in game. This is the only column you type in. |
| Current | What your monstats.txt gives right now, at the level shown. |
| Input | The number monstats.txt needs to reach your target. |
| Result | What that Input actually gives in game. Usually the same as your target. |

The rows cover HP, Defense, Experience, each attack's damage and attack rating (A1, A2, S1), and the three elemental slots. Rows the monster doesn't use are dimmed.

### The Level row

Each difficulty has a **Level** box at the top. It is filled from the area or from monstats.txt, and a small note beside it shows where the level came from, such as "area 85" or "file 67". You can type a different level at any time. Targets you haven't typed in yourself update to match the new level.

### Colours

- **Gold:** this value will change in monstats.txt when you apply.
- **Orange:** your exact target can't be reached at this level, so the nearest possible value is shown, with the difference beside it, such as "9025 (+25)". At high levels the game skips some values, so this is normal.
- **Red:** something needs attention. Common causes are a level that isn't in monlvl.txt, or a minimum that ends up higher than its maximum.

When the solver has a choice, it keeps your file's existing number if it already gives the target, and otherwise uses the smallest number that works.

## Elemental damage and durations

A monster has three elemental slots, El1 to El3. Each slot starts with an **El type** row holding a dropdown, followed by min damage, max damage and duration rows.

### Choosing an element

Pick the element from the dropdown, such as Fire, Cold or Poison. Changing it turns the dropdown gold and is written to monstats.txt when you apply. Beside it, each difficulty shows when the element triggers, for example "On A1 attacks, 50% of hits". An orange warning appears if the element has no attack mode set, because it will never trigger.

### Duration

Duration is measured in frames; 25 frames is one second, and the Result column shows the time in seconds. Only cold, poison, stun, burning, freeze and random use a duration.

For any other element the duration row is locked. If your file already has a duration for that slot, the Input shows **blank** in gold, and applying clears it from monstats.txt. Switch back to a duration element before applying and nothing is cleared.

### Poison and burning

Poison and burning deal their damage over time, so their targets work differently. The damage target is the **total** damage the game shows, such as "500 poison damage over 32 seconds", not a per-hit number.

- The game doubles the duration for poison and burning, so an in-game length of 400 frames needs 200 in monstats.txt. The page handles this for you.
- If you change the duration, the total damage target stays the same and the damage Input adjusts to fit.
- Hover a poison or burning result to see the exact rate and length the game will use.

## Linking difficulties and resetting targets

### Link difficulties

Tick **Link difficulties** when you want one monstats.txt value to carry across Normal, Nightmare and Hell. When you change a target in one difficulty, the other two get the same Input, and their targets update to show what that value gives at their own levels.

For example, with a Zombie linked, setting Normal Min HP to 20 gives an Input of 286 in all three difficulties. Nightmare then shows 1501 and Hell shows 11760.

- Linked targets have a dashed gold border. Hover one to see the shared Input.
- Whichever difficulty you edit last drives that stat.
- Changing a level keeps the link: the shared Input stays and the targets move.
- Min and max are linked separately, so set both when you change HP or damage, or the minimum can end up above the maximum.
- Unticking the box stops future copying but leaves existing values as they are.

### Fill from current and Clear targets

**Fill from current** resets every target to the monster's current in-game values from your file, undoing edits you haven't applied. **Clear targets** empties every target box. Both also remove any links.

## The AI row

Under the monster's name, the **AI** row shows which AI the monster uses and its AI settings for each difficulty: `aidel`, `aidist` and `aip1` to `aip8`.

- **Hover any column name** to see what it does. For `aip1` to `aip8` the description comes from monai.txt and depends on the AI selected, so load monai.txt to get them. Columns the AI doesn't use are dimmed.
- **Change the AI** with the dropdown. It lists every AI in monai.txt, or every AI your monstats.txt uses if monai.txt isn't loaded.
- **Edit any value** by typing in its box. Changed values turn gold and are written when you apply.

### What happens when you switch AI

1. The first time you pick a new AI for a monster, its settings are copied from the first monster in monstats.txt that uses that AI. The note beside the dropdown names that monster.
2. If you switch to an AI you already tried for this monster, your last settings for it come back.
3. Switching back to the monster's original AI restores its settings exactly as they were first read from the file.

This memory lasts until you reload the page.

## Monster properties

The **Properties** section, under the AI row, shows the extra properties a monster gets from MonProp.txt, written the way the game describes them. For example, a monster might show "All Resistances +25%" in Normal and "20% Chance to cast level 5 Nova on striking".

The monster's MonProp column in monstats.txt says which MonProp.txt row to use. Each property appears in its own row (prop1 to prop6), with one column per difficulty and its chance on the right. A blank chance in MonProp.txt counts as 100%.

To get full descriptions, load all five files on the **Monster properties** cards:

1. **MonProp.txt** lists the properties.
2. **properties.txt** turns each property code into the stats it sets.
3. **itemstatcost.txt** says how each stat is written, such as whether the number comes before or after the text.
4. **item-modifiers.json** supplies the English text.
5. **skills.txt** names the skills in chance-to-cast, aura and +skill properties.

The note beside **Properties** tells you which files are still missing. Until they are loaded, or when a property has no description in the game's files, the raw code and values are shown instead, such as `fade min 1, max 1`. Hover any property to see its code, par, min, max and chance.

The section is for reading only. To change a monster's properties, edit MonProp.txt itself.

## Applying and downloading

Changes happen in two steps: **Apply** writes the monster you're editing into the loaded monstats.txt, and **Download** saves that file to your computer.

- The Apply button shows how many changes are waiting, such as "Apply 4 changes". It counts stat values, element types, cleared durations, the AI and its settings.
- Tick **Also write Level columns** if you want the levels you entered saved to the monster's Level, Level(N) and Level(H) columns. It's off by default.
- If you pick another monster with changes still waiting, the page asks whether to apply them, discard them or keep editing.
- **Download monstats.txt** (or Ctrl+S) saves the file. Only the cells you changed differ; everything else, including line endings, stays byte-for-byte the same.
- Applied edits are kept in the browser between visits, but download before relying on them. The page warns you if you try to leave with changes you haven't applied.

**Forget files** removes every saved file and edit from this browser. Download first if you want to keep your work.

## How the numbers are calculated

Every stat follows one rule: the monstats.txt value is multiplied by a monlvl.txt factor for the monster's level and divided by 100, with any remainder dropped.

```latex
\text{in game} = \left\lfloor \frac{\text{monstats value} \times \text{monlvl factor}}{100} \right\rfloor
```

The solver uses the monlvl.txt L- columns, which are the ones single player and TCP/IP games use. Nightmare and Hell use the (N) and (H) versions of each column.

| Stat | monlvl.txt column |
| --- | --- |
| HP | L-HP |
| Defense | L-AC |
| Experience | L-XP |
| Attack rating | L-TH |
| All damage, elemental included | L-DM |

Durations are not scaled by level.

### Poison and burning

The scaled damage times 10 is the rate the game applies each frame, in 1/256ths of a point. The duration is doubled, and the total shown in game is the rate times the frames, divided by 256.

```latex
\text{total} = \left\lfloor \frac{\text{scaled damage} \times 10 \times \text{duration} \times 2}{256} \right\rfloor
```

For example, 5 scaled damage with a duration of 100 deals 5 × 10 × 200 ÷ 256 = 39 damage over 8 seconds.

## Troubleshooting

**The monster is weaker in game than the page says.** The most common cause is the level. Regular Nightmare and Hell monsters use the area's level, so load levels.txt and pick the right area. Also remember each monster rolls its HP between Min and Max; set both to the same value for a clean test.

**My changes don't show up in game at all.** Check that the downloaded file replaced the one your mod actually loads, in the right `data/global/excel` folder. A leftover monstats.bin, or another mod layer with its own monstats.txt, can override it.

**A result is orange.** At high levels the game can't produce every value, so the nearest one is used. The difference is shown next to the result.

**A result is red.** Check that the level is in monlvl.txt, and that each minimum isn't higher than its maximum.

**I can't edit a monster.** Summons and similar monsters with `noRatio` set take their stats from the skill that creates them, so the page locks them.

**A monster in the area list is greyed out.** It already appears in an earlier area. Pick it where it first appears, or use **Monster, then area** to choose a different area.

**Monster or area names show odd internal keys.** Load monsters.json and levels.json from `data/local/lng/strings` to get English names.

**A property shows a code like `res-all min 25, max 25` instead of text.** One of the five property files is missing; the note beside **Properties** names it. If all five are loaded, that property simply has no description in the game's files. A description that shows an internal key such as `strModFireResistance` means item-modifiers.json isn't loaded or doesn't have that key.

**These numbers don't match champions or uniques.** The page calculates base monsters. The game adds level bonuses and HP multipliers for champions and uniques, and extra HP for more players, on top.
