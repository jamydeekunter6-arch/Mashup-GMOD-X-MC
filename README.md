{
  "project": {
    "title": "Physgun Minecraft",
    "tagline": "Grab, freeze and throw anything in Minecraft with Garry's Mod's physgun.",
    "description": "Flat creative sandbox with a gravity/physics gun and a GMod-style prop spawn menu. Sounds and props are read from your own Garry's Mod install.",
    "license": "MIT",
    "remixAllowed": true,
    "players": "solo",
    "host": {"game": "minecraft-java", "role": "primary", "loader": "fabric", "build": "portable Prism Launcher instance, mmc-pack.json names fabric; mod in {minecraft-instance}/mods (see mashup_info skycraft)"},
    "companion": {"game": "garrys-mod", "role": "companion", "access": "{game:garrys-mod}", "rule": "never copy GMod files; read at runtime from the player's install"},
    "assumptions": ["Minecraft version chosen by builder from game_info", "Fabric chosen because Melty installs it itself"]
  },
  "sheet_tools": {
    "columns": ["id", "item", "use_hold", "use_release", "attack", "sneak_scroll", "range_blocks", "sound_ref", "status"],
    "rows": [
      ["physgun", "physgun (hotbar slot 1 at spawn)", "grab target, hold at distance", "drop", "freeze target in place", "scroll = distance, sneak+scroll = rotate", 24, "sounds.physgun_hold", "unverified"],
      ["gravgun_throw", "same item, second mode", "pick up", "throw with impulse", "punt", "-", 12, "sounds.throw", "unverified"]
    ]
  },
  "sheet_targets": {
    "columns": ["id", "kind", "grabbable", "freezable", "physics_body", "on_freeze", "status"],
    "rows": [
      ["block", "falling-block entity made from placed block", true, true, "gravity+collision", "convert back to block", "unverified"],
      ["mob", "any living entity", true, true, "velocity override", "NoAI + NoGravity", "unverified"],
      ["prop", "spawned GMod prop entity", true, true, "gravity+collision", "stay as entity", "unverified"]
    ]
  },
  "sheet_prop_menu": {
    "columns": ["id", "slot", "source_game", "source_path", "render", "status"],
    "rows": [
      ["menu_open", "key Q (rebindable)", "garrys-mod", "{game:garrys-mod}/garrysmod/ (VPK index read at runtime)", "grid of prop names", "unverified"],
      ["prop_spawn", "click entry", "garrys-mod", "models/props_*/ in the player's VPKs", "runtime-loaded mesh (MDL/VVD/VTX reader) or fallback cube with prop name", "RISK: mesh reader is the hardest piece"]
    ]
  },
  "sheet_sounds": {
    "columns": ["id", "source_game", "source_path", "trigger", "status"],
    "rows": [
      ["sounds.physgun_hold", "garrys-mod", "sound/weapons/physgun_*  (read from VPK at runtime)", "while holding", "unverified"],
      ["sounds.throw", "garrys-mod", "sound/weapons/physcannon/*", "on throw", "unverified"]
    ]
  },
  "sheet_world": {
    "columns": ["id", "setting", "value"],
    "rows": [
      ["world", "type", "superflat creative, daytime, no mobs spawning"],
      ["start_inventory", "slot 1", "physgun"],
      ["start_inventory", "keybind", "Q opens prop menu"]
    ]
  },
  "sheet_hooks": {
    "columns": ["id", "game_system", "fabric_hook", "status"],
    "rows": [
      ["use_item", "item use hold/release", "Item.usageTick / onStoppedUsing", "unverified"],
      ["attack", "left click with item", "AttackEntityCallback / AttackBlockCallback", "unverified"],
      ["scroll", "hotbar scroll while holding", "client mixin on Mouse.onMouseScroll", "unverified"],
      ["gmod_path", "locate GMod folder", "env var / melty.properties from launch arg {game:garrys-mod}", "unverified"]
    ]
  },
  "preflight_todo": [
    "Resolve every 'unverified' cell",
    "Confirm GMod VPK read path via game_info + a real install",
    "Decide mesh fallback if the MDL reader is not finished for v1",
    "Run one_click_check with recipe, entries and says (feature list)",
    "Real screenshot of physgun holding a block in the sandbox"
  ]
}
