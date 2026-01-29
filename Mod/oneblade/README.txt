================================================================================
                           ONEBLADE MOD - INSTALLATION GUIDE
================================================================================

ITEM GUID: fd0db742-6c8c-475e-983f-1594f99fc904

================================================================================
STEP 1: INSTALL THE MOD
================================================================================

1. Locate your KCD2 installation folder:
   - Usually: C:\Program Files (x86)\Steam\steamapps\common\KingdomComeDeliverance2

2. Create a "mods" folder if it doesn't exist:
   - [KCD2 Install]\mods\

3. Copy the entire "oneblade" folder into the mods folder:
   - [KCD2 Install]\mods\oneblade\

Final structure should be:
  [KCD2 Install]\mods\oneblade\
    ├── mod.manifest
    ├── Data\oneblade.pak
    └── Localization\English_xml.pak

================================================================================
STEP 2: ENABLE DEV MODE (for console testing)
================================================================================

1. Open Steam
2. Right-click "Kingdom Come: Deliverance II"
3. Select "Properties"
4. In "Launch Options" add: -devmode
5. Click OK

================================================================================
STEP 3: TEST IN GAME
================================================================================

Option A: Start New Game
- The OneBlade should appear in Henry's inventory at game start

Option B: Use Console Command (existing save)
1. Launch the game
2. Press ~ (tilde) to open console
3. Type: wh_cheat_addItem fd0db742-6c8c-475e-983f-1594f99fc904
4. Press Enter
5. Check your inventory for "OneBlade"

================================================================================
ITEM STATS
================================================================================

Name:           OneBlade
Type:           Longsword (Class 4)
Attack:         200
Defense:        280
Durability:     300 (Unbreakable)
Weight:         2.4 kg
Price:          5000 Groschen
Requirements:   STR 10, AGI 10
Charisma:       +25

================================================================================
TROUBLESHOOTING
================================================================================

Item doesn't appear:
- Check that modid "oneblade" matches in all files
- Verify file names have TWO underscores: item__oneblade.xml
- Check game log for errors (enable logging in game settings)

Console command doesn't work:
- Make sure -devmode is in launch options
- Check GUID is typed correctly (with dashes)

Localization shows "ui_nm_oneblade" instead of name:
- Verify English_xml.pak is properly created
- The .pak must contain text__oneblade.xml at root level

================================================================================
