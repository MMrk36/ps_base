# Pedro Scripts resource naming migration

Status: repository rename and source migration completed; all six foundation releases are published and the PS Base catalog is synchronized. PS Base validation and Windows EXE build pass. Live RedM verification is still pending.

## Official convention

All Pedro Scripts resource folders and GitHub repositories use `ps_` instead of `ps-`. The existing `ps_gunsmith` repository already followed this convention and remains unchanged.

## Repository rename map

| Previous repository | Current repository |
| --- | --- |
| `ps-base` | `ps_base` |
| `ps-core` | `ps_core` |
| `ps-database` | `ps_database` |
| `ps-characters` | `ps_characters` |
| `ps-inventory` | `ps_inventory` |
| `ps-notifications` | `ps_notifications` |
| `ps-permissions` | `ps_permissions` |
| `ps-menu` | `ps_menu` |
| `ps-medical` | `ps_medical` |
| `ps-doctor` | `ps_doctor` |
| `ps-medical-damage-probe` | `ps_medical_damage_probe` |
| `ps-propattach` | `ps_propattach` |
| `ps-deadmans-deck` | `ps_deadmans_deck` |
| `ps-pvp-keybind` | `ps_pvp_keybind` |
| `ps-gunsmith-menu` | `ps_gunsmith_menu` |
| `ps-ragdoll` | `ps_ragdoll` |
| `ps-minigame` | `ps_minigame` |
| `ps-progressbar` | `ps_progressbar` |
| `ps-weapons` | `ps_weapons` |

## Completed migration work

- Updated resource names, manifest dependencies and shared imports in the resource manifests.
- Updated cross-resource exports, resource-state checks and provider keys in the medical, doctor, character and weapons integrations, including PS Core's own module API, the PS-MENU demo, medical symptom notices, doctor medical-bag storage, weapon gunsmith permissions and the propattach packaging script.
- Updated PS Base submodule URLs and paths, including the six Git submodule entries.
- Updated PS Base catalog IDs, repository names, install paths and dependency IDs.
- Updated release metadata in the six currently catalogued foundation repositories.
- Updated the PS Base catalog schema and tests for underscore resource IDs. PS Base validation and Windows EXE build both pass after the migration.
- Updated the catalog synchronizer to ignore legacy releases whose metadata still identifies a hyphenated resource, then use newly published underscore-named releases. Existing ZIP assets remain on GitHub but are not installable through the migrated catalog.
- Updated the medical release package folder prefix.
- Renamed the bundled ragdoll and minigame directories to `standalone/ps_ragdoll` and `standalone/ps_minigame`, and aligned their manifests.
- Updated resource documentation, examples and the PS-MENU demo resource references. The `ps_progressbar`, `ps_gunsmith_menu` and `ps_gunsmith` repositories currently contain README-only placeholders rather than resource manifests or runtime code; `ps_gunsmith` already uses the underscore name.

## Compatibility decisions

Not every hyphenated string is a resource name. Existing network event namespaces (for example `ps-medical:*`, `ps-doctor:*`, `ps-menu:*` and `ps-minigames:*`) are preserved to avoid breaking integrations that listen for those events. Existing command names and ACE permission identifiers are also preserved. Historical release asset filenames remain hyphenated until new releases are published; the catalog synchronizer explicitly supports this transition.

## Verification still required

The GitHub source changes are committed, but they do not replace a live RedM test. Before production use:

1. Update server resource folder names to the new underscore names.
2. Check the startup order for dependencies such as `ps_core`, `ps_database`, `ps_ragdoll`, `ps_medical`, `ps_menu` and `ps_minigame`.
3. Start the resources and check for missing-resource, export and provider errors.
4. Test the medical/doctor, database/characters, weapons/database and menu integrations.
5. Confirm the six foundation releases and their underscore-named ZIP assets are published: `ps_core v0.1.1`, `ps_database v0.1.1`, `ps_characters v0.1.2`, `ps_inventory v0.1.1`, `ps_permissions v0.1.1` and `ps_notifications v0.1.1`. The PS Base catalog has been updated with these releases; verify installation from the PS Base GUI/CLI on a test machine.

Do not keep both old and new copies of the same resource running simultaneously.
