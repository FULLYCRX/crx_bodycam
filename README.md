# crx_bodycam (v1.0)
**Author:** I_AM_FULLYCRX / fullcrxstiano_7  
**Version:** 1.0.0

A high-fidelity, framework-agnostic Bodycam & Dispatch System for FiveM. 
Supports **QBCore**, **ESX**, **Ox Inventory**, and **Ox Lib**.

## Custom Panel View: Central Dispatch Panel
*   **Live Watch Dashboard:** Supervisors can view a real-time list of all active bodycams.
*   **Spectate System:** "Hack" into an officer's feed with a confirmation dialog.
*   **OneSync Support:** Automatically handles teleportation and scoping to ensure the target is visible.
*   **Multiple View Modes:** Configurable camera angles (Chest, Eyes/FPS, Third Person).
*   **Rank Restrictions:** Restrict panel access to specific grades (e.g., Sergeant+).

---

## Dependencies
1.  **[ox_lib](https://github.com/overextended/ox_lib)** (Required for the menu & notifications).
2.  **es_extended** OR **qb-core**.

---

## Installation Instructions

1.  **Download & Extract:** Put the `crx_bodycam` folder into your server's `resources` directory.
2.  **Config:** Open `config.lua` to set your Framework (`qb`/`esx`) and Inventory (`qb`/`esx`/`ox`).
3.  **Server Config:** Add the following to your `server.cfg` (Ensure ox_lib starts first):
    ```cfg
    ensure ox_lib
    ensure crx_bodycam
    ```

---

### Item Setup Additions

#### 1. Ox Inventory Setup (`ox_inventory/data/items.lua`)
Add this entry to your items list:
```lua
['bodycam'] = {
    label = 'AXON Bodycam',
    weight = 200,
    stack = false,
    close = true,
    client = { export = 'crx_bodycam.attemptToggle' }
},

['bodycam_terminal'] = {
    label = 'Dispatch Terminal',
    weight = 1000,
    stack = false,
    close = true,
    description = 'Secure tablet for viewing officer feeds.',
    client = { export = 'crx_bodycam.openPanel' } -- Optional if using item callback
},
```

#### 2. QBCore Inventory Setup (`qb-core/shared/items.lua`)
Add this entry to your items array:
```lua
['bodycam'] = {
    ['name'] = 'bodycam', 
    ['label'] = 'AXON Bodycam', 
    ['weight'] = 200, 
    ['type'] = 'item', 
    ['image'] = 'bodycam.png', 
    ['unique'] = true, 
    ['useable'] = true, 
    ['shouldClose'] = true, 
    ['description'] = 'Standard issue law enforcement body camera.'
},

['bodycam_terminal'] = {
    ['name'] = 'bodycam_terminal', 
    ['label'] = 'Dispatch Terminal', 
    ['weight'] = 1000, 
    ['type'] = 'item', 
    ['image'] = 'bodycampanel.png', 
    ['unique'] = false, 
    ['useable'] = true, 
    ['shouldClose'] = true, 
    ['description'] = 'Access the bodycam network.'
},
```

#### 3. ESX Database Item Configuration (SQL Script)
Run this SQL command in your database tool if using legacy ESX item database storage:
```sql
INSERT INTO `items` (`name`, `label`, `weight`) VALUES
('bodycam', 'Police Bodycam', 1),
('bodycam_terminal', 'Dispatch Terminal', 1);
```

## Features
* **/bodycamon** and **/bodycamoff** console commands.
* Automated real-time counter.
* Active screen layout rendering Date, Live Time, Identity Details, and Rank.

## Officer Commands 
* **/bodycamon** - Manually activate the camera (Requires item if configured).
* **/bodycamoff** - Deactivate the camera.
*  Using the bodycam item in inventory will toggle it on/off.

##  Dispatch / Supervisor Commands
* **/bodycampanel** - Opens the Live Feed Dashboard. Requirements: Must have the bodycam_terminal item (configurable) and meet the minimum job grade in config.lua.
* Controls: Click a name to view. Press [ESC] to terminate the feed and return to your original location.

## Configuration
* Advanced Configuration (config.lua) Rank Restrictions to limit who can see the panel,
* Config.WatchPanel.AllowedGrades.Example: Setting ['police'] = 2 means Grade 0 (Cadet) and 1 (Officer) cannot open the panel. Only Grade 2 (Sergeant) and higher can.
* Camera AnglesChange Config.WatchPanel.ViewMode:"chest" - Attaches to the spine (Standard Bodycam look)."eyes" - First Person view (Shakey, realistic)."third" - Over-the-shoulder view. 

## Troubleshooting
* {content: }"Script Error: ox_lib not found": You must install and start ox_lib."Access Denied": Check your job grade in the database database vs the number in config.lua.Camera is underground? 
* Ensure OneSync is enabled on your server. The script attempts to teleport you to the target to load their entity; if OneSync is off, this may fail.
