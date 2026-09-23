# Architecture

> Version: 0.2.0
> Status: draft

---

## 1. Technology Stack

| Concern | Decision |
|---|---|
| Language | C# |
| UI Framework | Avalonia (cross-platform desktop UI) |
| Architectural pattern | MVVM (Model-View-ViewModel) |
| Target platforms | Windows, Linux, macOS (desktop-first) |
| Mobile port | Not committed for this version; a future possibility, pending evaluation of Avalonia's mobile support |

---

## 2. Data & Persistence

- **Format:** JSON, stored as plain, human-readable files (see RNF03 in `requirements.md`).
- **File naming convention** (see glossary in `requirements.md`):
  - System file: `{system name}.structure.json`
  - Character sheet file: `{system name}.{character name}.character.json`
- **No permanent database.** Any in-memory or temporary storage (e.g., caching PDF search indexes or computed formula results) is allowed, but nothing is persisted outside the JSON files.
- **Atomic writes** are required to avoid data corruption on failure (see RNF11).

---

## 3. Module Boundaries

Per RNF06, the system should keep the following concerns separated:

- **PDF handling** — local import, text search, and opening in an external reader.
- **Persistence** — reading/writing/validating the JSON files described above.
- **Rules engine** — formula evaluation, dice rolls, actions, and progression.
- **UI rendering** — dynamic rendering driven by tags and fields (MVVM views/view models).
- **Localization** — translation lookup and culture switching (see section 4).

---

## 4. Localization (i18n)

Per RNF07, the interface must be translatable. Instead of a relational database (over-engineered for a flat key-value lookup), translations are handled the same way as everything else in the project: **plain JSON files**, consistent with RNF03.

### 4.1 File format and naming

- Location: `/locales/` (bundled defaults) plus a user-writable folder (e.g. `%appdata%/{app}/locales/` or platform equivalent) for imported/downloaded packs.
- Naming convention: `{locale}.lang.json` (e.g. `en-US.lang.json`, `pt-BR.lang.json`).
- Content: a flat (or lightly nested) key-value dictionary, with positional placeholders for dynamic values:

```json
{
  "ui.button.attack": "Attack",
  "ui.button.save": "Save",
  "sheet.field.strength": "Strength",
  "action.roll.result": "Roll result: {0}"
}
```

### 4.2 Behavior

- **Built-in vs. imported packs are treated identically.** At least `en-US` and `pt-BR` ship with the app; any additional `{locale}.lang.json` dropped into the user locales folder is picked up the same way — no installer or rebuild required.
- **Fallback is per-key, not per-file.** If an imported pack is missing a key, the service falls back to the base locale (`en-US`) for that key only, so a partial community translation never breaks a screen.
- **Validation.** Imported language packs go through the same schema validation pipeline as system/character files (RF37): expected keys, string types, and placeholder consistency (e.g. `{0}`) are checked before a pack is accepted.
- **Isolation.** Like system and character sheet files, a language pack must never bundle the rulebook PDF or system data — it is a purely presentational asset (consistent with RN01/RN06).
- **Runtime switching.** Exposed via an `ILocalizationService` (see below), so the user can change language without restarting the app.

### 4.3 Service shape

```
ILocalizationService
 ├─ CurrentCulture: string                 // e.g. "pt-BR"
 ├─ AvailableCultures: IEnumerable<CultureInfo>   // scans /locales/
 ├─ Translate(key: string, args?: object[]) : string
 └─ SetCulture(culture: string) : void     // raises a change notification
```

This is a **Service**, not part of the Model/View/ViewModel trio — it is injected into ViewModels, which expose translated strings as bindable properties. On Avalonia, this is typically paired with a custom `MarkupExtension` (e.g. `{loc:Tr ui.button.attack}`) so bound `.axaml` strings re-evaluate automatically when `SetCulture` is called, without manually reloading views.

---

## Notes

This document should be expanded as concrete architectural decisions are made during development (e.g., how modules communicate, how the rules engine parses and evaluates formulas, how the PDF reader is integrated, and how schema validation is implemented).
