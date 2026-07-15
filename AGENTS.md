# ProjectNautic Agent Rules

- Use `$maintain-modpack-design` whenever adding, modifying, moving, or removing mods, configurations, resource packs, data packs, scripts, quests, shaders, manifests, or other pack content.
- Before completing such work, scan the pack, update `docs/design/`, accept the documented inventory baseline, and verify that no pending changes remain.
- Keep machine-generated evidence under `docs/design/_generated/`; never replace human design rationale with a raw file inventory.
- Ignore runtime noise such as logs, downloads, saves, caches, and native libraries unless the user explicitly asks to document it.
