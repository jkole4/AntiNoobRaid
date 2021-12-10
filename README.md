# Introduction
Anti Noob Raid prevents doing damage to entities placed by a new player.
For this plugin to work, Playtime Tracker is needed to monitor how long players are on the server.
Time period inside which players cannot get raided is configurable in config file, by default it is 43200 (seconds).

## Permissions

```bash
This plugin uses Oxide's permission system. 
To assign a permission, use oxide.grant <user or group> <name or steam id> <permission>. 
To remove a permission, use oxide.revoke <user or group> <name or steam id> <permission>.
```

antinoobraid.admin - gives player permission to command /refunditem

antinoobraid.noob - gives player permanent noob protection status

## Chat Commands

### Admin
/refunditem add - adds the item you are holding to the list of items that will get refunded (in case the item is a raid tool)

/refunditem remove - removes the item you are holding from the list of items that will get refunded (in case the item is a raid tool)

/refunditem list - lists all the items that are currently set to refund

/refunditem clear - clears the list of items set to refund

/refunditem all - sets all raid tools to get refunded (C4, satchel, beancan, f1 and all 2 types of rockets (hv & normal))

### Players

/checknew - allows player to check if the entity is placed by a new player to avoiding losing explosives (needs to be looking at the building/entity)

## Console and Chat Commands (Admin Only)

(/) antinoob wipe playerdata - wipes player data from the file

(/) antinoob wipe attempts- wipes player raid attempts from the file

(/) antinoob wipe all - wipes the whole data file

(/) antinoob removenoob [SteamID] - makes the player non noob (use this in case you notice player is not a noob)

(/) antinoob addnoob [SteamID] - makes the player noob (use this in case you notice player is really... noob...)

## Entity Whitelist

In config file there are already 5 placeholder for entity whitelist, to add an entity to the list you need to replace the Placeholder with the short prefab of the entity you want to whitelist, you can find that by typing debug.lookingat in client console ("Ent: sleepingbag_leather_deployed").

If you want to make the entity destroyable everywhere around the world you set the true/false property to true and if you want the entity to be destroyable when it's outside of TC range and owner of entity is not authorized to the TC set it to false.

## Configuration

```bash
The settings and options can be configured in the AntiNoobRaid file under the config directory. 
The use of a JSON editor or validation site such as jsonlint.com is recommended to avoid formatting issues and syntax errors.
```

```json
{
  "Allow clan members to destroy each others entities (Rust:IO Clans)": true,
  "Allow team members to destroy each others entities": true,
  "Allow twig to be destroyed even when owner is noob": false,
  "Ignore twig when calculating base ownership (prevents exploiting)": true,
  "Check full ownership of the base instead of only one block": true,
  "Kill fireballs when someone tries to raid protected player with fire (prevents lag)": true,
  "Time (seconds) after which noob will lose protection (in-game time)": 43200,
  "Days of inactivity after which player will be raidable": 3.0,
  "Notify player on first connection with protection time": true,
  "Use game tips to send first connection message to players": true,
  "Show message for not being able to raid": true,
  "Show time until raidable": false,
  "Prevent new players from raiding": true,
  "Remove protection from all team members when a member tries to raid": true,
  "Remove protection from all clan members when a member tries to raid": false,
  "Remove noob status of a raider on raid attempt": true,
  "Remove noob status of a raider who is manually marked as a noob on raid attempt": true,
  "Refund explosives": true,
  "Refunds before player starts losing explosives": 4,
  "List of entities that can be destroyed even if owner is a noob, true = destroyable everywhere (not inside of owners TC range)": {
    "Placeholder1": true,
    "Placeholder2": true,
    "Placeholder3": true,
    "Placeholder4": true,
    "Placeholder5": true
  },
  "User data refresh interval (seconds)": 30,
  "Show structure has no owner in console": false,
  "Enable Logs (logs can be found in /oxide/logs/antinoobraid)": true
}

```

## Notes
For infinite refunding set the "Refunds before player starts losing explosives to zero.

For `"Refund explosives"` to work you need to set `"Remove noob status of a raider on raid attempt"` to false
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
