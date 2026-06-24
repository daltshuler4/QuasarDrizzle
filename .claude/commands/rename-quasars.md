# /rename-quasars
# Replaces quasar_01–06 placeholder folder names with real target names from MAST.
# Updates all directories and docs. Always asks for approval before renaming anything.

Replace placeholder folder names with real target names.

To use this command, provide the full mapping:
  quasar_01 → <real target name>
  quasar_02 → <real target name>
  quasar_03 → <real target name>
  quasar_04 → <real target name>
  quasar_05 → <real target name>
  quasar_06 → <real target name>

Steps:
1. Show the complete list of renames that will be made (all directories affected
   in both data/raw/ and data/processed/ including aligned/ and drizzled/ subdirs)
2. Wait for explicit user approval of the full mapping before touching any files
3. After approval:
   a. Rename all folders in data/raw/
   b. Rename all folders in data/processed/ (including subdirs)
   c. Update the target table in docs/quasars.md with real names and any known
      coordinates or identifiers
   d. Update the pipeline state table in HANDOFF.md with real names

Do not rename anything until the user has confirmed the complete mapping.
If the user provides fewer than 6 names, ask for the missing ones before proceeding.
