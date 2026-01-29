# OneBlade Mod - Project Context for Claude

> **Read this file at the start of every session.**
> This file contains project context, decisions, and current status.

## Project Overview

**Game:** Kingdom Come: Deliverance II v1.5.2
**Mod Name:** OneBlade
**Mod ID:** `oneblade` (lowercase, no numbers - required by KCD2)

### Goal
Create a custom weapon item called "OneBlade" that can be:
1. Obtained from a merchant in Troskovice (starting village)
2. Later: Have custom model and textures

### Current Phase
**Phase 1:** Basic item with borrowed assets (model/icon from existing sword)

### Item GUID
**`fd0db742-6c8c-475e-983f-1594f99fc904`**

---

## Key Technical Decisions

### Naming Convention (CRITICAL)
- **modid:** `oneblade`
- All PTF files: `tablename__oneblade.xml` (TWO underscores)
- Folder name must match modid exactly

### Asset Strategy (Phase 1)
- Reuse existing longsword model: `manmade/weapons/swords_long/sermiri_long_sword_guild.cgf`
- Reuse existing icon: `sermiry_longSwordGuild`
- Reuse existing scabbard: `Scabbard_LongSword04_m02`

### Item Placement
- Primary: Merchant inventory in Troskovice
- Backup: Console command `wh_cheat_addItem <GUID>`
- Alternative: Henry's starting inventory

---

## File Structure

**Mod files location in repo:** `Mod/oneblade/`

```
Mod/oneblade/                              <- Copy this folder to [KCD2]/mods/
├── mod.manifest
├── README.txt                             <- Installation instructions
├── Data/
│   └── Libs/
│       └── Tables/
│           └── Item/
│               ├── item__oneblade.xml
│               └── InventoryPreset__oneblade.xml
└── Localization/
    └── text__oneblade.xml                 <- MUST be packed into English_xml.pak!
```

---

## Reference Documentation

### Authoritative Sources (in priority order)
1. **KnowledgeBase/YouTrack/** - Official Warhorse documentation (LOCAL)
2. **KnowledgeBase/KCD2_Modding_Guide.md** - Compiled reference guide
3. **KnowledgeBase/VideoTutorial.docx** - Community tutorial notes
4. **Reference/** - Working mod examples (rose_equipment, z_radzigsword)

### Key YouTrack Documents
- `Walkthroughs/Adding a new Item.docx` - Step-by-step item creation
- `Modding - Game Data/Inventory presets.docx` - Shop/NPC inventory system
- `Modding - Game Data/Localization.docx` - Text/name localization
- `TechnicalOverview/StructureofaMod/` - Mod structure and manifest

---

## Current Status

### Completed
- [x] Set up knowledge base with YouTrack documentation
- [x] Analyzed reference mods (rose_equipment, z_radzigsword)
- [x] Created technical reference guide
- [x] Determined mod structure and file naming
- [x] Create mod folder structure
- [x] Generate unique GUID for OneBlade item
- [x] Create item__oneblade.xml
- [x] Create InventoryPreset__oneblade.xml (adds to Henry's starting inventory)
- [x] Create localization files

### In Progress
- [ ] Pack localization into English_xml.pak (user task - needs 7-Zip)
- [ ] Test in-game

### Future (Phase 2)
- [ ] Create custom 3D model
- [ ] Create custom textures
- [ ] Create custom icon
- [ ] Create custom scabbard (optional)

---

## Important Notes

### PTF Naming Rules
- File: `tablename__modid.xml` (TWO underscores)
- modid: lowercase letters and underscores ONLY (no numbers!)
- modid must match across: folder name, mod.manifest, all PTF files

### Inventory Preset Patching
- Full support from patch 1.3+
- Can add to existing merchant inventories by patching their preset name

### Testing
- Enable dev mode: Add `-devmode` to Steam launch options
- Console: Press `~` key
- Add item: `wh_cheat_addItem <GUID>`

---

## Session Notes

### Session 1 (2026-01-29)
- Initial project setup
- Downloaded and organized YouTrack documentation
- Read official docs: Adding new Item, Inventory Presets, Localization, Mod Structure
- Created knowledge base and project roadmap
- Created all mod files:
  - mod.manifest (modid: oneblade)
  - item__oneblade.xml (GUID: fd0db742-6c8c-475e-983f-1594f99fc904)
  - InventoryPreset__oneblade.xml (adds to Henry's inventory)
  - text__oneblade.xml (name + description)
- Next: User packs localization, tests in-game

---

*Update this file at the end of each session with progress and decisions.*
