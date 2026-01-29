# Kingdom Come: Deliverance II Modding Knowledge Base

> **Game Version:** 1.5.2
> **Last Updated:** January 2026
> **Project:** OneBlade Mod

This document serves as a comprehensive reference for modding Kingdom Come: Deliverance II, specifically focused on creating and adding custom items (weapons, equipment) to the game.

---

## Table of Contents

1. [Official Resources](#official-resources)
2. [Required Tools](#required-tools)
3. [Mod Structure](#mod-structure)
4. [Item Definition (item.xml)](#item-definition-itemxml)
5. [Localization](#localization)
6. [Inventory Presets](#inventory-presets)
7. [3D Models and Textures](#3d-models-and-textures)
8. [Blacksmith Recipes (Optional)](#blacksmith-recipes-optional)
9. [Perks for Recipes (Optional)](#perks-for-recipes-optional)
10. [Character Components (Scabbards)](#character-components-scabbards)
11. [Getting Items In-Game](#getting-items-in-game)
12. [Packing and Testing](#packing-and-testing)
13. [Reference Examples](#reference-examples)

---

## Official Resources

### Warhorse YouTrack Knowledge Base
Main hub: https://warhorse.youtrack.cloud/articles/KM-A-1/Modding-Kingdom-Come-Deliverance-2

| Article | URL |
|---------|-----|
| Main Modding Page | https://warhorse.youtrack.cloud/articles/KM-A-1 |
| **Adding a new Item** | https://warhorse.youtrack.cloud/articles/KM-A-17/Adding-a-new-Item |
| Structure of a Mod | https://warhorse.youtrack.cloud/articles/KM-A-3/Structure-of-a-Mod |
| The Modding Tools | https://warhorse.youtrack.cloud/articles/KM-A-55/The-Modding-Tools |
| Installing Mods | https://warhorse.youtrack.cloud/articles/KM-A-56/Installing-mods |
| Publishing a Mod | https://warhorse.youtrack.cloud/articles/KM-A-58/Publishing-a-mod |
| Technical Overview | https://warhorse.youtrack.cloud/articles/KM-A-36/Technical-Overview |

### Community Resources
- **Community Wiki:** https://modding.wiki/en/kingdomcomedeliverance2/mod-development
- **PTF Documentation:** https://modding.wiki/en/kingdomcomedeliverance2/mod-development/fundamentals/ptf
- **Adding Custom Weapon Guide (MAJOR76):** https://modding.wiki/en/kingdomcomedeliverance2/mod-development/disciplines/3d/Addingcustomweapon
- **Steam Workshop:** https://steamcommunity.com/workshop/about/?appid=1771300
- **KCD2 Modding Hub:** https://modskcd2.com/kingdom-come-deliverance-2-modding-hub/

---

## Required Tools

### 1. Official Modding Tools (Steam)
- Install "Kingdom Come Deliverance 2 Modding Tools" from Steam Library (enable "Tools" filter)
- **IMPORTANT:** Keep file path short to avoid tool issues
- Run `KCD2_mod > Tools > ModdingWorkspaceSetup > WorkSpaceSetup.exe` as Administrator
- Choose **System Link (S)** to auto-update with game patches

### 2. Archive Manager
- **7-Zip** (recommended) or WinRAR
- Required to open/extract `.pak` files

### 3. Text Editor
- **VS Code** or **Notepad++** for XML editing

### 4. KCD2 Pack Tool (Laughing Man)
- Download from GitHub
- Adds "Pack Mods" context menu option to compress mod folders into `.pak` format

### 5. Blender Toolkit (For 3D Work)
- Community plugin supporting import/export of KCD2 models
- Configuration required:
  - **Data Directory:** Set to base game's Data folder
  - **Texture Path:** Folder for exported textures
  - **RC Path:** Link to `KCD2_mod > Tools > RC > RC.exe`

### 6. Texture Tools
- **Kamzik's Texture Tool:** Converts streamed `.dds` files to editable formats
- **NVIDIA Texture Tools Exporter:** For Photoshop or standalone DDS editing

### 7. KCD Asset Finder (by Alier)
- Search engine for game assets by keyword across all `.pak` files

### 8. Warbox (Advanced Tool)
- GitHub: https://github.com/vawser/Warbox
- **Table Editor:** Search and modify configuration table data
- **Text Editor:** Modify text/localization content
- **PTF Generation:** "Save Patch File" saves only entries that differ from base game
- **Packaging:** Creates PAK files ready for game loading, auto-generates mod.manifest
- Requirements: Windows 7-11 (64-bit), .NET Core 7.0, Vulkan 1.3-compatible GPU

### 9. ModForge
- GitHub: https://github.com/Destuur/ModForge
- GUI tool for reading, editing, and exporting XML-based game files
- Supports: Perks, Buffs, Debuffs, Localizations
- Auto-generates mod folder structure and mod.manifest
- Exports directly to .pak format

---

## Mod Structure

### Folder Layout
```
mods/
└── your_mod_name/
    ├── mod.manifest                          # Required - mod metadata
    ├── Data/
    │   ├── Libs/
    │   │   ├── Tables/
    │   │   │   ├── item/
    │   │   │   │   └── item__your_mod_name.xml         # Item definitions (PTF)
    │   │   │   │   └── InventoryPreset__your_mod_name.xml  # Inventory additions
    │   │   │   ├── Character/
    │   │   │   │   └── CharacterComponent__your_mod_name.xml  # Scabbards, etc.
    │   │   │   ├── minigame/
    │   │   │   │   └── BlacksmithRecipes__your_mod_name.xml   # Crafting recipes
    │   │   │   └── rpg/
    │   │   │       ├── perk__your_mod_name.xml         # Recipe perks
    │   │   │       └── perk_script__your_mod_name.xml
    │   │   └── UI/
    │   │       └── Textures/
    │   │           └── Icons/
    │   │               └── Items/
    │   │                   └── your_icon.dds           # Item icon (128x128)
    │   └── Objects/
    │       └── manmade/
    │           └── weapons/
    │               └── swords_long/
    │                   ├── your_weapon.cgf             # 3D model
    │                   ├── your_weapon.mtl             # Material
    │                   ├── your_weapon_diff.dds        # Diffuse texture
    │                   ├── your_weapon_ddna.dds        # Normal + gloss
    │                   ├── your_weapon_spec.dds        # Specular
    │                   └── your_weapon_weapon_mask.dds # Weapon mask
    └── Localization/
        └── English_xml.pak                   # Contains text_ui_your_mod_name.xml
```

### mod.manifest Template
```xml
<?xml version="1.0" encoding="utf-8"?>
<kcd_mod>
  <info>
    <name>Your Mod Display Name</name>
    <modid>your_mod_name</modid>
    <description>Description of your mod</description>
    <author>Your Name</author>
    <version>1.0</version>
    <created_on>2026-01-29</created_on>
  </info>
  <supports>
    <version>1.*</version>
  </supports>
</kcd_mod>
```

### Patched Table Files (PTF) Naming Convention

PTF allows targeted file modifications instead of overwriting entire files. This enhances compatibility, enabling multiple mods to alter the same file provided they don't modify the same lines.

**CRITICAL NAMING RULES:**
1. File must be named `tablename__modid.xml` (TWO underscores between table name and modid)
2. The modid must match EXACTLY across:
   - Mod folder name
   - `<modid>` in mod.manifest
   - PTF file suffix
3. Modid restrictions:
   - **Lowercase letters only**
   - **No numbers**
   - **Underscores allowed**

**Examples:**
- `item__oneblade.xml` ✓
- `rpg_param__my_first_mod.xml` ✓
- `item_oneblade.xml` ✗ (single underscore)
- `item__OneBlade.xml` ✗ (uppercase)
- `item__oneblade123.xml` ✗ (numbers)

**How PTF Works:**
When the game loads, it processes all Table files including those from mods. Mod Table Files take precedence over built-in ones, enabling overrides of vanilla properties. PTF mods won't conflict unless they edit the exact same lines for the same entries.

---

## Item Definition (item.xml)

### File: `Data/Libs/Tables/item/item__your_mod_name.xml`

### MeleeWeapon Schema
```xml
<?xml version="1.0" encoding="us-ascii"?>
<database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          name="barbora"
          xsi:noNamespaceSchemaLocation="item.xsd">
  <ItemClasses version="8">
    <MeleeWeapon
      Attack="160"                    <!-- Base attack damage -->
      AttackModStab="0.9"             <!-- Stab damage multiplier -->
      AttackModSlash="1.0"            <!-- Slash damage multiplier -->
      AttackModSmash="0.3"            <!-- Smash damage multiplier -->
      Class="4"                       <!-- Weapon class (see below) -->
      SubClass="11"                   <!-- Weapon subclass -->
      Defense="235"                   <!-- Defense value -->
      MaxStatus="250"                 <!-- Durability -->
      StrReq="14"                     <!-- Strength requirement -->
      AgiReq="8"                      <!-- Agility requirement -->
      IsBreakable="true"              <!-- Can break -->
      BrokenItemClassId="aeb13096-..."  <!-- GUID of broken item variant -->
      Visibility="1"
      Conspicuousness="1"
      Charisma="20"                   <!-- Charisma bonus -->
      SocialClassId="0"
      WealthLevel="0"
      MaxQuality="4"                  <!-- Max quality level (1-4) -->
      UiSound="ui_inv_item_weapon_sword"
      Clothing="Scabbard_LongSword01" <!-- Scabbard component name -->
      IconId="your_icon"              <!-- Icon filename without extension -->
      UIInfo="ui_in_your_item"        <!-- Localization key for description -->
      UIName="ui_nm_your_item"        <!-- Localization key for name -->
      Model="manmade/weapons/swords_long/your_weapon.cgf"
      Material="manmade/weapons/swords_long/your_weapon.mtl"
      Weight="4.2"                    <!-- Weight in kg -->
      Price="30466"                   <!-- Base price in Groschen -->
      FadeCoef="1"
      VisibilityCoef="13.0153751"
      Id="YOUR-GUID-HERE"             <!-- Unique GUID (use generator) -->
      Name="your_internal_name"       <!-- Internal reference name -->
    />
  </ItemClasses>
</database>
```

### Weapon Classes
| Class | Type |
|-------|------|
| 1 | Short Sword |
| 3 | Axe |
| 4 | Long Sword |
| 5 | Mace |
| 7 | Polearm |
| 16 | Hunting Sword/Falchion |

### Generating GUIDs
Use online GUID generator or PowerShell: `[guid]::NewGuid().ToString()`

---

## Localization

### File: `Localization/English_xml.pak`
Contains: `text_ui_your_mod_name.xml`

### Localization XML Format
```xml
<Table>
  <Row>
    <Cell>ui_nm_your_item</Cell>
    <Cell>Your Item Display Name</Cell>
    <Cell>Your Item Display Name</Cell>
  </Row>
  <Row>
    <Cell>ui_in_your_item</Cell>
    <Cell>Description of your item that appears in inventory.</Cell>
    <Cell>Description of your item that appears in inventory.</Cell>
  </Row>
</Table>
```

### Creating Localization .pak
1. Create the XML file with proper naming (`text_ui_modid.xml`)
2. Use 7-Zip to create a `.zip` archive
3. Rename `.zip` to `.pak`
4. Place in `Localization/` folder

### Multi-Language Support
Create separate `.pak` files:
- `English_xml.pak`
- `Czech_xml.pak`
- `German_xml.pak`
- etc.

---

## Inventory Presets

### File: `Data/Libs/Tables/item/InventoryPreset__your_mod_name.xml`

Used to add items to:
- Player's starting inventory
- Merchant inventories
- Container inventories

### Adding to Player Inventory (Start of Game)
```xml
<?xml version="1.0" encoding="us-ascii"?>
<database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          name="barbora"
          xsi:noNamespaceSchemaLocation="InventoryPreset.xsd">
  <InventoryPresets version="2">
    <InventoryPreset Name="inventory_player_henry">
      <PresetItem Name="keyring" />
      <PresetItem Name="your_internal_name" Amount="1" Quality="4" Condition="1" />
    </InventoryPreset>
  </InventoryPresets>
</database>
```

### Adding to Merchant Inventory
```xml
<InventoryPresets version="2">
  <!-- Create custom inventory preset -->
  <InventoryPreset Name="inventory_your_weapons" Mode="All" Health="1">
    <PresetItem Name="your_weapon_name" Amount="1" Quality="3" Condition="1" />
  </InventoryPreset>

  <!-- Add to specific merchant (e.g., Kuttenberg weaponsmith) -->
  <InventoryPreset Name="inventory_shop_weaponSmithLeflirska" Mode="All" Health="1">
    <InventoryPresetRef Name="inventory_your_weapons" />
  </InventoryPreset>
</InventoryPresets>
```

### Known Merchant Inventory Names
| Location | Inventory Name |
|----------|---------------|
| Kuttenberg Weaponsmith | `inventory_shop_weaponSmithLeflirska` |
| (More to be documented) | |

### PresetItem Attributes
- `Name` - Internal item name from item.xml
- `Amount` - Quantity
- `Quality` - Quality level (1-4)
- `Condition` - Durability (0.0-1.0, where 1.0 = full)

### Note on InventoryPreset PTF
InventoryPreset files may have limited PTF support. Consider using the **IPM Tool (InventoryPreset Merger Tool)** for better compatibility.

---

## 3D Models and Textures

### File Types
| Extension | Description |
|-----------|-------------|
| `.cgf` | Static/Physics 3D model (weapons, props) |
| `.skin` | Character/wearable model (armor, clothing) |
| `.mtl` | Material definition file |
| `_diff.dds` | Diffuse/albedo texture (color) |
| `_ddna.dds` | Normal map + gloss combined |
| `_spec.dds` | Specular map |
| `_weapon_mask.dds` | Weapon-specific mask texture |

### Texture Workflow (Editing Existing)

1. **Extract textures** using KCD Asset Finder or manually from `.pak`
2. **Convert to editable format** using Kamzik's Texture Tool
   - Enable "Separate Gloss Map" option
   - Output: `.tif` files
3. **Edit in image editor** (Photoshop, GIMP)
4. **Convert back to DDS** using Resource Compiler (`RC.exe`)
   - Drag `.tif` onto `RC.exe`
   - File suffix determines conversion (e.g., `_diff`, `_ddna`)
5. **Place in correct path** matching game structure

### Model Workflow (Blender)

#### Importing
- **From Pack:** Plugin scans game files directly
- **Loose File:** Manual extraction required
  - For CGF: Must also extract `.cgfm` metadata file

#### Key Points
- Helpers (empties) define hand placement: `SLT_0` (left), `SLT_1` (right)
- Physics Proxies define collision
- **NEVER apply transforms to Helper points**
- After modifying, recreate collision proxies with exact original names

#### Exporting
- **CGF:** Export C Engine → generates `.dae` then `.cgf`
- **Skin:** Export KCD2 → generates `.skin`
- **Rename if needed:** Exporter may append `_mesh` - rename to match original

### Creating Item Icons
1. Create icon at appropriate size (typically 128x128 or 256x256)
2. Save as `.tif` in: `Data/Libs/UI/Textures/Icons/Items/`
3. Convert to DDS using RC.exe
4. **IMPORTANT:** Icons must use **BC7 compression format** if exporting directly to DDS
5. Icon filename uses `_icon` suffix (e.g., `oneblade_icon.dds`)
6. In item.xml, reference WITHOUT the `_icon` suffix: `IconId="oneblade"`

### Modeling Tips (from MAJOR76)
- Start modeling on top of existing models to understand slot locations
- Be careful with grip positioning - affects how character holds weapon
- When exporting, sharp edge info is lost - add supporting edges to prevent shading artifacts
- Copy `bloodrust_mask` & `scratches_dt` textures from existing weapons (universal to all swords)

### Folder Paths for Assets
```
Objects/manmade/weapons/swords_long/     # Long swords
Objects/manmade/weapons/swords_short/    # Short swords
Objects/manmade/weapons/axes/            # Axes
Objects/manmade/weapons/maces/           # Maces
Objects/manmade/weapons/long_weapons/    # Polearms
Objects/manmade/weapons/hunting_swords/  # Hunting swords/falchions
Objects/characters/humans/male/clothing/scabbard/longsword/  # Scabbards
Libs/UI/Textures/Icons/Items/           # Item icons
```

---

## Blacksmith Recipes (Optional)

### File: `Data/Libs/Tables/minigame/BlacksmithRecipes__your_mod_name.xml`

Allows players to craft your item at a blacksmith.

```xml
<?xml version="1.0" encoding="us-ascii"?>
<database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          name="barbora"
          xsi:noNamespaceSchemaLocation="../database.xsd">
  <BlacksmithRecipes version="1">
    <BlacksmithRecipe
      Id="r_your_weapon"
      UIIcon="your_icon"
      Category="LongSword"              <!-- Category: ShortSword, LongSword, Axe, etc. -->
      MinSkillLevel="1"                 <!-- Required blacksmithing skill -->
      WorkpieceId="longsword"           <!-- Base workpiece type -->
      PerkId="YOUR-PERK-GUID">          <!-- Associated perk GUID -->
      <UIName StringName="ui_nm_your_item" Text="Your Weapon" LoadedOn="12.7.2023 10:49:56" />
      <UIDesc StringName="ui_in_your_item" Text="Description" LoadedOn="21.3.2024 14:19:57" />
      <Ingredients>
        <!-- Use ItemClassId GUIDs from game's item.xml -->
        <BlacksmithRecipeIngredient ItemClassId="54f297f8-62c0-41b5-9ab4-892c7475fc6a" Amount="1" />
      </Ingredients>
      <Products>
        <BlacksmithRecipeProduct ItemClassId="YOUR-ITEM-GUID" MinimalQuality="70" />
      </Products>
    </BlacksmithRecipe>
  </BlacksmithRecipes>
</database>
```

### Common Ingredient GUIDs (from reference mods)
| Item | GUID |
|------|------|
| Steel | `54f297f8-62c0-41b5-9ab4-892c7475fc6a` |
| Iron | `92aa6120-028e-48ee-8ed1-1c5f91afaa26` |
| Frankfurt Steel | `3c1c0ae2-731e-40c1-a917-024fb3f000da` |
| Toledo Steel | `4a4da84c-f12a-4bc8-94dc-a7d8d76788ea` |
| Deer Skin | `a1dda25f-3a35-4376-b198-4e5173c742a8` |
| Cow Skin | `e5ac7c40-263d-4fba-8c00-343e9b112aef` |
| Exotic Wood | `87a568f2-79f7-415f-a690-9a04c4585455` |
| Coin Sword Pommel | `1fe0e850-e07d-45f0-ade0-26f030a63da4` |
| Eight-sided Pommel | `4f7a7d02-b8cb-4bcc-9b3e-edc1992ee580` |
| Ordinary Sword Guard | `d01f5606-5bba-42c1-9a48-b065e7a92ad7` |
| Reinforced Sword Guard | `1c933935-d4b3-4884-8228-a4cde0c3a96d` |
| Horned Sword Guard | `6bfe50b1-dafb-4bf7-a1d9-1f61feb3ac53` |
| Straight Sword Guard | `bc09cb07-fb9d-45be-8f55-d42b999c6341` |
| Copper | `8b7515e1-21fb-4c18-b3da-86fabb5025bd` |

---

## Perks for Recipes (Optional)

Required if using blacksmith recipes.

### File: `Data/Libs/Tables/rpg/perk__your_mod_name.xml`
```xml
<?xml version="1.0" encoding="us-ascii"?>
<database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          name="barbora"
          xsi:noNamespaceSchemaLocation="../database.xsd">
  <perks version="1">
    <perk
      autolearnable="false"
      perk_id="YOUR-PERK-GUID"
      perk_name="BS recipe - r_your_weapon"
      visibility="0" />
  </perks>
</database>
```

### File: `Data/Libs/Tables/rpg/perk_script__your_mod_name.xml`
```xml
<?xml version="1.0" encoding="us-ascii"?>
<database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          name="barbora"
          xsi:noNamespaceSchemaLocation="../database.xsd">
  <perk_scripts version="1">
    <perk_script
      perk_id="YOUR-PERK-GUID"
      perk_editor_name="BS recipe - r_your_weapon" />
  </perk_scripts>
</database>
```

---

## Character Components (Scabbards)

### File: `Data/Libs/Tables/Character/CharacterComponent__your_mod_name.xml`

Required if creating a custom scabbard for your weapon.

```xml
<?xml version="1.0" encoding="utf-8"?>
<database>
  <CharacterComponents>
    <Component Name="Male">
      <DerivedComponents>
        <Component Name="Clothing">
          <DerivedComponents>
            <Component Name="Scabbard">
              <DerivedComponents>
                <Clothing Name="LongSword">
                  <DerivedComponents>
                    <Clothing Name="Scabbard_YourWeapon"
                              DropModel="path/to/dropmodel.cgf">
                      <Elements>
                        <SkinElement
                          EquipmentPart="belt"
                          Model="path/to/scabbard.skin"
                          Material="path/to/scabbard.mtl" />
                        <!-- Joint elements for physics simulation -->
                      </Elements>
                      <Features>
                        <Feature Name="ScabbardBase" Material="Material_Name" />
                        <!-- Additional features -->
                      </Features>
                    </Clothing>
                  </DerivedComponents>
                </Clothing>
              </DerivedComponents>
            </Component>
          </DerivedComponents>
        </Component>
      </DerivedComponents>
    </Component>
  </CharacterComponents>
</database>
```

---

## Getting Items In-Game

### Method 1: Console Command (Easiest for Testing)

1. **Enable Dev Mode:**
   - Steam → Right-click KCD2 → Properties → General
   - Add `-devmode` to Launch Options

2. **Open Console:** Press `~` key in-game

3. **Add Item Command:**
   ```
   wh_cheat_addItem YOUR-ITEM-GUID
   ```
   Example:
   ```
   wh_cheat_addItem BF81CF7F-7806-48B2-8D05-6142540F5A0E
   ```

### Method 2: Player Starting Inventory
- Use `InventoryPreset__modid.xml` with `inventory_player_henry`
- Item appears when starting new game

### Method 3: Merchant Inventory
- Use `InventoryPreset__modid.xml` to add to shop inventories
- Player can purchase from merchants

### Method 4: Blacksmith Crafting
- Create recipe + required perks
- Player can forge at blacksmith
- Requires recipe document item for learning

### Other Useful Console Commands
```
wh_cheat_money X          # Add X Groschen
wh_sys_NoSavePotion = 1   # Save without Saviour Schnapps
```

---

## Packing and Testing

### Creating the .pak File

1. **Organize mod structure** as shown above
2. **Right-click mod folder** → "Pack Mods" (if KCD2 Pack Tool installed)
3. **Or use 7-Zip:**
   - Select contents of Data folder
   - Create `.zip` archive
   - Rename to `.pak`

### Testing
1. Place mod folder (with `mod.manifest`) in game's `mods/` directory
2. **Or** pack into `.pak` and place in `mods/ModName/Data/modname.pak`
3. Launch game
4. Check item exists via console or merchant

### Debugging Tips
- Check game logs for errors
- Verify PTF naming (double underscore!)
- Verify GUIDs are unique
- Verify file paths match exactly

---

## Reference Examples

### Rose Equipment Mod Structure
```
rose_equipment/
├── mod.manifest
├── Data/
│   └── rose_equipment.pak
│       ├── Libs/Tables/item/item__rose_equipment.xml
│       ├── Libs/Tables/item/InventoryPreset__rose_equipment.xml
│       ├── Libs/Tables/minigame/BlacksmithRecipes__rose_equipment.xml
│       ├── Libs/Tables/rpg/perk__rose_equipment.xml
│       ├── Libs/Tables/rpg/perk_script__rose_equipment.xml
│       ├── Libs/UI/Textures/Icons/Items/*.dds
│       └── Objects/manmade/weapons/*/*.cgf, *.mtl, *.dds
└── Localization/
    └── English_xml.pak
```

### Radzigsword Mod Structure
```
z_radzigsword/
├── mod.manifest
├── Data/
│   └── radzigsword.pak
│       ├── Libs/Tables/item/item__z_Radzigsword.xml
│       ├── Libs/Tables/Character/CharacterComponent__z_Radzigsword.xml
│       ├── Libs/Tables/minigame/BlacksmithRecipes__z_Martins_swords.xml
│       ├── Libs/UI/Textures/Icons/Items/Radzigsword_icon.dds
│       └── Objects/manmade/weapons/swords_long/Radzigsword.*
└── Localization/
    ├── English_xml.pak
    ├── Czech_xml.pak
    └── Chineses_xml.pak
```

---

## Quick Start Checklist for OneBlade Mod

- [ ] Create mod folder structure: `mods/oneblade/`
- [ ] Create `mod.manifest` with modid `oneblade`
- [ ] Generate unique GUID for item
- [ ] Create `item__oneblade.xml` with MeleeWeapon definition
- [ ] Create localization XML for name/description
- [ ] Pack localization into `English_xml.pak`
- [ ] Choose item acquisition method:
  - [ ] Console command (testing)
  - [ ] Player inventory (InventoryPreset)
  - [ ] Merchant inventory (InventoryPreset)
- [ ] For custom model/textures:
  - [ ] Create/modify 3D model (.cgf)
  - [ ] Create/modify textures (_diff, _ddna, _spec, _weapon_mask .dds)
  - [ ] Create material file (.mtl)
  - [ ] Create icon (.dds, placed in Icons/Items/)
- [ ] Pack into .pak file
- [ ] Test in-game

---

## External Tool Links

| Tool | Description | Link |
|------|-------------|------|
| KCD2 Pack Tool | Compresses mod folders to .pak | GitHub (Laughing Man) |
| Blender Toolkit | Import/export KCD2 models | GitHub |
| Kamzik's Texture Tool | Convert streamed DDS files | Nexus Mods |
| KCD Asset Finder | Search game assets by keyword | Community tool (Alier) |
| IPM Tool | InventoryPreset merger | Nexus Mods |
| Warbox | Advanced table/text editor with PTF | https://github.com/vawser/Warbox |
| ModForge | GUI XML editor and mod packager | https://github.com/Destuur/ModForge |

---

## Troubleshooting

### Common Issues

**Item doesn't appear in game:**
- Verify PTF naming (TWO underscores: `item__modid.xml`)
- Check modid matches across mod.manifest and all PTF files
- Ensure GUID is unique and properly formatted
- Check file paths match game structure exactly

**Textures appear wrong:**
- Verify file suffix matches type (`_diff`, `_ddna`, `_spec`)
- Check texture dimensions are power of 2
- Ensure DDS compression format is correct

**Console command doesn't work:**
- Verify `-devmode` is in Steam launch options
- Use full GUID with dashes
- Check for typos in GUID

**Mod conflicts with other mods:**
- Use PTF format instead of full file replacement
- Check if other mods edit same table entries
- Consider using IPM Tool for InventoryPreset compatibility

---

*This document is maintained as part of the OneBlade Mod project for KCD2.*
*Last updated: January 2026*
