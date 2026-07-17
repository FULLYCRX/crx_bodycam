# CRX Bodycam & Panel
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

# Preview
<img width="1920" height="1080" alt="20260623090312_1" src="https://github.com/user-attachments/assets/fc21153e-c02f-4934-b362-f62758da3cf4" />
<img width="1920" height="1080" alt="20260623085056_1" src="https://github.com/user-attachments/assets/ceb7a180-6194-4d12-b2ae-1727a376e885" />
<img width="1920" height="1080" alt="20260623084537_1" src="https://github.com/user-attachments/assets/c0518617-9248-4208-acd6-6bbdfb939829" />
<img width="1920" height="1080" alt="20260623084523_1" src="https://github.com/user-attachments/assets/31bb2542-94aa-495f-8acf-47a1cbb0b1dc" />
<img width="1920" height="1080" alt="20260623084516_1" src="https://github.com/user-attachments/assets/7e1b33c9-cab9-4442-9d96-4b2ef892cf06" />
<img width="1920" height="1080" alt="20260623084510_1" src="https://github.com/user-attachments/assets/45f69c6e-203d-4c89-a379-d499ed056eb6" />
<img width="1920" height="1080" alt="20260623084505_1" src="https://github.com/user-attachments/assets/64d538a4-5d3b-41d5-be34-676783e84fd3" />
<img width="1920" height="1080" alt="20260623084459_1" src="https://github.com/user-attachments/assets/a362f80a-30d6-46a6-a647-81bd6a3d8b00" />

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

# Support
[I_AM_FULLYCRX Official Discord](https://discord.gg/6QchSKDY7j)
[Iron Justice Roleplay Official Discord](https://discord.gg/YJXZagSzh7)
