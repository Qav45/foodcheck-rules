# foodcheck-rules

Ingredient rules for the **Food Check** Android app. The app fetches `rules.json` once a day
and falls back to the copy compiled into the APK if this file is unreachable or malformed —
so editing this file updates the app with no reinstall.

## The bar for each list

**`avoid`** — demonstrated harm to a real person at amounts a real person actually eats.
"Banned somewhere" is *not* enough on its own; several bans are precautionary or procedural
leftovers. Substances whose only harm evidence is extreme animal dosing belong in `warning`.

**`warning`** — worth knowing about, but not proven to hurt people at normal amounts. These
keep a food out of "Eat Every Day" and nothing more.

Every entry must answer **"how much before it actually hurts me?"** in `howMuch`, in plain words.

## Fields

| Field | Required | Meaning |
|---|---|---|
| `id` | yes | Stable identifier |
| `name` | yes | Shown as the card heading |
| `tags` | yes (may be empty) | Open Food Facts additive tags, e.g. `en:e924` |
| `pattern` | yes | Case-insensitive regex matched against the ingredients text |
| `why` | yes | One or two plain sentences |
| `howMuch` | yes | The harm dose, in plain words |
| `bannedIn` | no | Where it is actually illegal |

## Editing safely

The app rejects the whole file and keeps its built-in copy if `version` is missing or not
newer, `avoid` is empty, any entry is missing a required field, or any `pattern` is not a
valid regex. Bump `version` on every change or the app will ignore it.

Validate before committing:

```bash
python -c "import json;json.load(open('rules.json'))"
```
