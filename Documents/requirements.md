# Scope and Requirements — RPG Character Sheet Manager

> Initial scope and core requirements document.
> Version: 0.2.0
> Status: draft

---

## Table of Contents

1. [Overview](#1-overview)
2. [Goals](#2-goals)
3. [Scope](#3-scope)
4. [Glossary](#4-glossary)
5. [Functional Requirements](#5-functional-requirements)
6. [Non-Functional Requirements](#6-non-functional-requirements)
7. [Business Rules](#7-business-rules)
8. [Main Use Cases](#8-main-use-cases)
9. [Constraints and Assumptions](#9-constraints-and-assumptions)
10. [Out of Scope](#10-out-of-scope)
11. [Initial Roadmap](#11-initial-roadmap)

---

## 1. Overview

The project is a cross-platform desktop system for **managing tabletop RPG character sheets**, focused on **generality and modularity**. The system does not implement fixed rules for any specific RPG; instead, it provides a **generic engine** driven by declarative definitions created by the user, allowing any RPG system to be modeled within the program.

The system is designed for **individual, free, and open-source use**, working **offline** and storing data in open, human-readable formats.

---

## 2. Goals

### 2.1 Main goal

Provide an **adaptable, general-purpose tool** capable of managing character sheets for any RPG system, letting the user build their own **rules interpreter** declaratively.

### 2.2 Specific goals

- Allow creation, editing, importing, and exporting of **system projects** (interpreters).
- Allow creation and management of **multiple character sheets** per system.
- Provide **dynamic UI rendering** based on tags and fields defined by the user.
- Provide a **rules engine** capable of evaluating formulas, executing actions, rolling dice, and applying progression.
- Allow **importing local PDFs** of the system's rulebook, for simplified reference.
- Ensure that exported files are **self-contained**, portable, and never include the rulebook itself.

### 2.3 Secondary goals

- Be cross-platform (Windows, Linux, macOS).
- Be extensible without requiring programming skills from the average user.
- Keep data in an open, versioned format.
- Make it easy to share systems and character sheets between users.

---

## 3. Scope

### 3.1 In scope

- Creation, editing, importing, and exporting of system projects.
- Creation of interpreters with tags, fields, resources, items, actions, rules, and progression.
- Creation of multiple character sheets per system.
- Free-form editing of character sheets.
- Exporting and importing character sheets.
- Importing a local PDF and simplified reading/text search.
- Offline functionality.
- Free and open distribution.

### 3.2 Out of scope (this phase)

- Real-time or cloud-based collaborative editing.
- Execution of arbitrary scripts (C#, Lua, JS) inside the interpreter.
- Including the rulebook PDF in exported system or character sheet files.
- Virtual Tabletop (VTT), maps, real-time initiative, or multiplayer.
- An official store or central repository of systems.
- Cross-device synchronization.

---

## 4. Glossary

| Term | Definition |
|---|---|
| **System / Project** | File that defines how an RPG works within the program. Does not contain the rulebook. Stored as `{system name}.structure.json`. |
| **Interpreter** | Set of declarative definitions (tags, fields, rules, actions, progression) that describe a system. |
| **Tag** | Metadata applied to fields, items, or actions that drives the system's behavior. |
| **Field** | Unit of data on a character sheet (number, text, resource, item, etc.). |
| **Derived sub-field** | A field automatically generated from a base field via a tag-defined formula (e.g., a `Strength` field tagged as an attribute automatically generates `Strength.Mod`). |
| **Character Sheet** | Instance of a character based on a System. Stored as `{system name}.{character name}.character.json`. |
| **Action** | Executable operation combining rolls, formulas, and conditions. |
| **Event** | Trigger that fires effects (on item use, on level up, etc.). |
| **Rules Engine** | Component that evaluates formulas, conditions, and actions based on the system definition and the character sheet data. |
| **Rulebook PDF** | Local file owned by the user, used only for reading/reference. Never embedded in exports. |

---

## 5. Functional Requirements

### 5.1 System Management

| ID | Requirement | Priority |
|---|---|---|
| RF01 | The user must be able to create a new system project. | High |
| RF02 | The user must be able to edit an existing system project. | High |
| RF03 | The user must be able to import a system project from a file. | High |
| RF04 | The user must be able to export a system project to a file. | High |
| RF05 | The user must be able to delete a system project. | Medium |
| RF06 | The user must be able to duplicate an existing system project. | Low |
| RF07 | The system must version projects, allowing migration between versions. | Medium |

### 5.2 Interpreter Definition

| ID | Requirement | Priority |
|---|---|---|
| RF08 | The user must be able to create custom tags. | High |
| RF09 | The user must be able to associate behaviors with tags (rendering, formulas, hooks). | High |
| RF10 | The user must be able to create fields (attributes, resources, text, items, etc.). | High |
| RF11 | The user must be able to apply tags to fields. | High |
| RF12 | The user must be able to create formulas for derived values. | High |
| RF12.1 | A tag applied to a base field may automatically generate one or more derived sub-fields computed from that field (e.g., a `Strength` field tagged as an attribute automatically generates `Strength.Mod`). | High |
| RF13 | The user must be able to create executable actions. | High |
| RF14 | The user must be able to define conditions for display and execution. | Medium |
| RF15 | The user must be able to define progression (level, XP, milestones, etc.). | Medium |
| RF16 | The user must be able to define events and their effects. | Medium |

### 5.3 Character Sheet Management

| ID | Requirement | Priority |
|---|---|---|
| RF17 | The user must be able to create multiple character sheets per system. | High |
| RF18 | The user must be able to freely edit a character sheet's values. | High |
| RF19 | The system must automatically recalculate derived values. | High |
| RF20 | The user must be able to export character sheets. | High |
| RF21 | The user must be able to import character sheets. | High |
| RF22 | The user must be able to delete character sheets. | Medium |
| RF23 | The user must be able to duplicate character sheets. | Medium |

### 5.4 Rules Engine

| ID | Requirement | Priority |
|---|---|---|
| RF24 | The system must evaluate mathematical and logical formulas. | High |
| RF25 | The system must execute dice rolls with modifiers. | High |
| RF26 | The system must support outcome ranges in actions. | Medium |
| RF27 | The system must apply event effects (onUse, onEquip, onLevelUp, etc.). | Medium |
| RF28 | The system must detect cycles in formulas and prevent infinite loops. | High |

### 5.5 Dynamic Interface

| ID | Requirement | Priority |
|---|---|---|
| RF29 | The interface must render fields dynamically based on the system definition. | High |
| RF30 | The interface must adapt the visual component based on the field's tag. | High |
| RF31 | The interface must show/hide fields based on defined conditions. | Medium |

### 5.6 PDF Import and Reading

| ID | Requirement | Priority |
|---|---|---|
| RF32 | The user must be able to import a local PDF of the rulebook. | Medium |
| RF33 | The user must be able to search text within the imported PDF. | Medium |
| RF34 | The user must be able to open the PDF in an external reader. | Medium |
| RF35 | The system must not include the PDF in exports. | High |

### 5.7 Persistence

| ID | Requirement | Priority |
|---|---|---|
| RF36 | The system must store data locally in an open format. System files are stored as `{system name}.structure.json`; character sheets are stored as `{system name}.{character name}.character.json`. | High |
| RF37 | The system must validate imported files against a schema. | High |
| RF38 | The system must perform simple manual backups (file copies). | Medium |

---

## 6. Non-Functional Requirements

| ID | Requirement |
|---|---|
| RNF01 | Cross-platform: Windows, Linux, and macOS. |
| RNF02 | Offline-first functionality. |
| RNF03 | Data in open format (JSON and/or ZIP). |
| RNF04 | The interpreter must not execute arbitrary code. |
| RNF05 | Low resource consumption, suitable for individual use. |
| RNF06 | Modularity: PDF handling, persistence, and rules engine kept separate. |
| RNF07 | Accessible and translatable interface (i18n). |
| RNF08 | Easy manual backup of files by the user. |
| RNF09 | Free and open-source codebase. |
| RNF10 | Formula response time under 100ms on typical character sheets. |
| RNF11 | Persistence must not corrupt data on failure (atomic writes). |

---

## 7. Business Rules

| ID | Rule |
|---|---|
| RN01 | The system file must not contain the rulebook PDF. |
| RN02 | The character sheet references the system by `id` and `version`. |
| RN03 | Tags are declarative, not executable code. |
| RN04 | Formulas use a restricted language, with no access to files, network, or the OS. |
| RN05 | If the system's version changes, the character sheet must go through migration or display a warning. |
| RN06 | Exporting a system does not export the PDF. Exporting a character sheet does not export the system or the PDF. |
| RN07 | Imported files must be validated before use. |
| RN08 | A system project must have a unique identifier and a version. |

---

## 8. Main Use Cases

### UC01 — Create a new system
**Actor:** User
**Flow:** creates project → defines metadata → defines tags → defines fields → saves.

### UC02 — Add tag and derived field
**Actor:** User
**Flow:** creates an `attribute` tag with a modifier formula → creates a `Strength` field → applies the tag → the system automatically generates `Strength.Mod`.

### UC03 — Create a resource with a formula
**Actor:** User
**Flow:** creates a `HP` field → applies the `resource` tag → defines a maximum-value formula → the character sheet displays a current/max bar.

### UC04 — Create an action with a roll
**Actor:** User
**Flow:** creates an `Attack` action → defines the formula `1d20 + Strength.Mod` → defines outcomes → saves.

### UC05 — Create a character sheet
**Actor:** User
**Flow:** selects a system → creates a character sheet → fills in values → the system computes derived values.

### UC06 — Export a system
**Actor:** User
**Flow:** selects a system → exports `{system name}.structure.json` → shares the file.

### UC07 — Import a system
**Actor:** User
**Flow:** imports a `{system name}.structure.json` file → the system validates the schema → the system becomes available.

### UC08 — Import a PDF and look up a rule
**Actor:** User
**Flow:** imports a local PDF → searches for a term → reads the excerpt.

### UC09 — Export and import a character sheet
**Actor:** User
**Flow:** exports `{system name}.{character name}.character.json` → opens it on another machine → the system requests the corresponding system file, if missing.

---

## 9. Constraints and Assumptions

### 9.1 Constraints

- The system must not depend on online services to function.
- The system must not execute arbitrary code provided by the user in the interpreter.
- The system must not include the rulebook PDF in exported files.
- The system must run on machines with modest resources.

### 9.2 Assumptions

- The user has basic knowledge of the RPG system they want to model.
- The user is willing to build the interpreter manually, without programming.
- The rulebook PDF is legally obtained by the user.
- The user has access to a local file system for backups.

---

## 10. Out of Scope

The following items are **not** part of this version's scope:

- VTT, maps, grids, or tokens.
- Real-time initiative or multiplayer.
- Collaborative or cloud-based editor.
- Execution of arbitrary scripts inside the interpreter.
- An official store or repository of systems.
- Automatic cross-device synchronization.
- Integration with external RPG APIs.
- Automatic character sheet generation from the PDF.
- Optical character recognition (OCR) of the PDF.

---

## 11. Initial Roadmap

| Milestone | Description |
|---|---|
| **M0** | Documentation and schema definition (requirements, architecture, DSL). |
| **M1** | System CRUD: create, edit, save, and load. |
| **M2** | Dynamic character sheets: tag-based rendering and simple calculations. |
| **M3** | Rules engine: formulas, conditions, rolls, and actions. |
| **M4** | Importing and exporting systems and character sheets. |
| **M5** | PDF import and text search. |
| **M6** | Polish: versioning, migration, accessibility, i18n. |

---

## Final Notes

This document is a **living draft**. As development progresses, requirements may be refined, removed, or added. It is recommended to keep this file under version control alongside the source code.

Planned complementary documents:


