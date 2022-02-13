# Introduction
Anti Noob Raid prevents doing damage to entities placed by a new player for a certain amount of time.
For this plugin to work, Playtime Tracker is needed to monitor how long players are on the server.
Time period inside which players cannot get raided is configurable in the config file, by default it is 43200 (seconds).

## Permissions

```bash
This plugin uses Oxide's permission system. 
To assign a permission, use oxide.grant <user or group> <name or steam id> <permission>. 
To remove a permission, use oxide.revoke <user or group> <name or steam id> <permission>.
```

antinoobraid.admin - gives player permission to command /refunditem & /checkname

antinoobraid.noob - gives player permanent noob protection status

## Chat Commands

### Admin
/refunditem add - adds the item you are holding to the list of items that will get refunded (in case the item is a raid tool)

/refunditem remove - removes the item you are holding from the list of items that will get refunded (in case the item is a raid tool)

/refunditem list - lists all the items that are currently set to refund

/refunditem clear - clears the list of items set to refund

/refunditem all - sets all raid tools to get refunded (C4, satchel, beancan, f1 and all 2 types of rockets (hv & normal))

/checkname looking - check a deployed item and get its name for "Entity Settings"

/checkname holding - check the weapon your holding and get its name for "Weapon Settings"

### Players

/checknew - allows player to check if the entity is placed by a new player to avoiding losing explosives (needs to be looking at the building/entity)

## Console and Chat Commands (Admin Only)

(/) antinoob wipe playerdata - wipes player data from the file

(/) antinoob wipe attempts- wipes player raid attempts from the file

(/) antinoob wipe all - wipes the whole data file

(/) antinoob removenoob [SteamID] - makes the player non noob (use this in case you notice player is not a noob)

(/) antinoob addnoob [SteamID] - makes the player noob (use this in case you notice player is really... noob...)

## Entity Settings

In the config file there are already some items in both list for example's. Too add an entity to one of the list you need to add the short prefab name of the entity you want to add to the list, you can find that by typing debug.lookingat in client console or /checkname looking (Should look something like this "sleepingbag_leather_deployed").

This will make the entity destroyable everywhere around the world by everyone. If you want an item to be destroyable but not if the player is new check out `Notes` below

## Weapon Settings

In the config file there is already a item in the list for a example. Too add an weapon to the list you need to add the short prefab name of the weapon you want to add to the list, you can find that by typing /checkname holding in chat while holding the weapon (Should look something like this "rocket_smoke").

## Notes
For infinite refunding set the `"Refunds before player starts losing explosives"` to zero.

For `"Refund explosives"` to work you need to set `"Remove noob status of a raider on raid attempt"` to false

`"Kill fireballs when someone tries to raid protected player with fire"` won't kill fireballs from Explosive Ammo & Incendiary Ammo as the hitinfo.InitiatorPlayer is Returning nothing with fire created with those items

If using `"Anti Ladder and Twig"` you can disable `"Ignore twig when calculating base ownership (prevents exploiting)"` option to protect Twig as that plugin prevents the exploit with Twig

 Fire produced by Explosive Ammo & Incendiary Ammo from Team/Clan members won't damage Team/Clan structures if protected. This is due to the hitinfo.InitiatorPlayer returning nothing with the fire. the Bullet Damage & Explosive Damage will still damage.
 
 `"List of entities that can be destroyed without losing noob prtection, true = destroyable everywhere"` This option was added so noobs can destroy Certain Items if desired without losing protection. Best to add Storage Containers here As it will allow noobs to destroy boxes in Decaying Bases but wont allow other player's to destroy there's
 
 `"Show time until raidable only to owners"` This will allow the Base Owner to check there protection time on only there base! Clan/Team member will also be able Too if enabled

`"List of Weapons/Tools that won't trigger player to lose noob protection"`  dose not support Explosive 5.56 Rifle Ammo, Incendiary 5.56 Rifle Ammo, Incendiary Pistol Bullet, 12 Gauge Incendiary Shell and Fire Arrow. This is due the fire / explosive part of the ammo returns null for some infomation needed.
 
 `"Save data on Server Save"` If disabled it will save every 60 seconds or what is in `"Save interval (seconds)"`
 
 ## Configuration

```bash
The settings and options can be configured in the AntiNoobRaid file under the config directory. 
The use of a JSON editor or validation site such as jsonlint.com is recommended to avoid formatting issues and syntax errors.
```

```json
{
  "Main Settings": {
    "Time (seconds) after which noob will lose protection (in-game time)": 43200,
    "Days of inactivity after which player will be raidable": 3.0,
    "Remove noob status of a raider on raid attempt": true,
    "Remove noob status of a raider who is manually marked as a noob on raid attempt": true
  },
  "Other Settings": {
    "Allow Patrol Helicopter to damage noob structures (This will allow players to raid noobs with Patrol Helicopter)": true,
    "Ignore twig when calculating base ownership (prevents exploiting)": true,
    "Check full ownership of the base instead of only one block": true,
    "Kill fireballs when someone tries to raid protected player with fire (prevents lag)": true
  },
  "Team & Clan Settings": {
    "Enable 'Clan' Support (Allow clan members to destroy each others entities & Remove protection from clan members when a member tries to raid)": true,
    "Enable 'Team' Support (Allow team members to destroy each others entities & Remove protection from team members when a member tries to raid)": true
  },
  "Refund Settings": {
    "Refund explosives": true,
    "Refunds before player starts losing explosives": 4
  },
  "Manage Messages": {
    "Notify player on first connection with protection time": true,
    "Use game tips to send first connection message & lost protection to players": false,
    "Show message for not being able to raid": true,
    "Show time until raidable": false,
    "Show time until raidable only to owners": true
  },
  "Entity Settings": {
    "List of entities that can be destroyed even if owner is noob, true = destroyable everywhere": {
      "ShortPrefabName": "Common Name",
      "beartrap": "Snap Trap",
      "landmine": "Landmine"
    },
    "List of entities that can be destroyed without losing noob protection, true = destroyable everywhere": {
      "ShortPrefabName": "Common Name",
      "campfire": "Camp Fire"
    }
  },
    "Weapon Settings": {
    "List of Weapons/Tools that won't trigger player to lose noob protection": {
      "ShortPrefabName": "Common Name",
      "rocket_smoke": "Smoke Rocket WIP!!!!"
    }
  },
  "Advance Settings": {
    "User data refresh interval (seconds)": 30,
    "Save interval (seconds)": 60,
    "Save data on Server Save": true,
    "Show structure has no owner in console": false,
    "Enable Logs (Logs can be found in /oxide/logs/antinoobraid)": true
  }
}

```

## API
# IgnorePlayer 

 Should be called when a player is being added to an event or something
 
**(bool) IgnorePlayer(object o)**

Example: `AntiNoobRaid.Call("IgnorePlayer", "71661298069130333");`

Returns true if user was successfully added to ignore list and false if not

Accepts: BasePlayer, ulong & string

# UnIgnorePlayer
**UnIgnorePlayer** - Should be called when a player is being removed from an event or something

**(bool) UnIgnorePlayer(object o)**

Example: `AntiNoobRaid.Call("UnIgnorePlayer", "71661298069130333");`

Returns true if user was successfully removed from ignore list and false if not

Accepts: BasePlayer, ulong & string