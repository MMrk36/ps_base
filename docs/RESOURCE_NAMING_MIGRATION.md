# Pedro Scripts resource naming migration

Status: repository rename and source migration completed; live RedM verification is still required.

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
- Updated cross-resource exports, resource-state checks and provider keys in the medical, doctor, character and weapons integrations.
- Updated PS Base submodule URLs and paths, including the six Git submodule entries.
- Updated PS Base catalog IDs, repository names, install paths and dependency IDs.
- Updated release metadata in the six currently catalogued foundation repositories.
- Updated the PS Base catalog schema and tests for underscore resource IDs.
- Updated the catalog synchronizer to accept the existing hyphenated release metadata and asset names while emitting underscore dependency IDs. This keeps existing published releases installable until new releases are published.
- Updated the medical release package folder prefix.
- Renamed the bundled ragdoll and minigame directories to `standalone/ps_ragdoll` and `standalone/ps_minigame`, and aligned their manifests.
- Updated resource documentation, examples and the PS-MENU demo resource references.

## Compatibility decisions

Not every hyphenated string is a resource name. Existing network event namespaces (for example `ps-medical:*`, `ps-doctor:*`, `ps-menu:*` and `ps-minigames:*`) are preserved to avoid breaking integrations that listen for those events. Existing command names and ACE permission identifiers are also preserved. Historical release asset filenames remain hyphenated until new releases are published; the catalog synchronizer explicitly supports this transition.

## Verification still required

The GitHub source changes are committed, but they do not replace a live RedM test. Before production use:

1. Update server resource folder names to the new underscore names.
2. Check the startup order for dependencies such as `ps_core`, `ps_database`, `ps_ragdoll`, `ps_medical`, `ps_menu` and `ps_minigame`.
3. Start the resources and check for missing-resource, export and provider errors.
4. Test the medical/doctor, database/characters, weapons/database and menu integrations.
5. Run the PS Base validation and catalog tests, then verify that an existing published release still downloads under its legacy asset filename.

Do not keep both old and new copies of the same resource running simultaneously.
