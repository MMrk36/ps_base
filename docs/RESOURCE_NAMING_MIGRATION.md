# Pedro Scripts resource naming migration

Status: preparation and dependency audit. No runtime identifiers have been changed in this commit.

## Official convention

All Pedro Scripts RedM resource folder names and GitHub repository names use `ps_` instead of `ps-`.

Examples:
- `ps-medical` -> `ps_medical`
- `ps-menu` -> `ps_menu`
- `ps-core` -> `ps_core`

## Repository rename map

| Existing repository | Target repository |
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

The existing `ps_gunsmith` repository already follows the new convention and is not renamed.

## Runtime references that must migrate together

The repository and resource folder rename must be coordinated with:
- `fxmanifest.lua` `dependencies` entries.
- Shared script imports such as `@ps-core/...`.
- Cross-resource exports such as `exports['ps-menu']`.
- Resource-name lookups and resource-state checks.
- Installer catalog IDs, repository URLs, submodule paths, release asset names and update metadata.
- Documentation, examples, server configuration snippets and CI/release workflows.

## Nested resources and duplicate copies

The current repository trees also contain nested resource copies:
- `ps-medical/standalone/ps-ragdoll` declares resource name `ps-ragdoll`.
- `ps-doctor/standalone/ps-minigames` declares resource name `ps-minigames`.

These must be reconciled with the standalone repositories before changing names. Do not silently rename or overwrite either copy; compare their contents and establish the canonical source first.

## Compatibility safeguards

Do not blindly replace every occurrence of `ps-` with `ps_`. Some strings are stable public interfaces rather than resource names, including network event namespaces, command names, permission/ACE identifiers, log labels, release history and historical documentation. Preserve these unless a deliberate compatibility migration is defined.

Where an event or command namespace is intentionally changed, keep a temporary legacy listener/alias and document its removal policy. Resource exports and manifest imports must point to the new resource name after the folder rename.

## Safe migration order

1. Rename GitHub repositories while preserving repository history.
2. Update `ps_base/.gitmodules`, submodule paths, catalog IDs, repository URLs, release metadata and installer tests as one coordinated change.
3. Reconcile nested copies against canonical standalone repositories.
4. Rename resource folders and update manifests, imports, exports, resource-state checks and integration references across all repositories.
5. Update documentation and examples; retain explicit upgrade notes for existing server installations.
6. Search every repository for old resource identifiers and run available automated tests.
7. Smoke-test dependency startup order and cross-resource exports on a RedM test server before declaring the migration complete.

## Current limitation

The connected GitHub operations available to this workspace can inspect repositories and commit file changes, but do not expose the GitHub repository-settings rename operation. Repository renames therefore need to be performed in GitHub's repository settings (or through an authorized GitHub API credential with repository administration access) before runtime references are switched. Until then, keeping the current manifests intact avoids breaking existing installations.
