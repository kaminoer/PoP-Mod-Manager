# PoP Mod Manager Manifest Generator

A small, offline C++ console app for preparing `pmm_manifest.json` for PoP Mod
Manager repositories.

## Quick start

1. Copy `PMMManifestGenerator.exe` into your local repository folder alongside
   the JTMods, then double-click it. You can also drag a repository folder onto
   the EXE to select that folder.
2. For each new mod, confirm the suggested stable ID, then choose **Mod/Fix**,
   **Classic/OpenJade/Both**, and an optional image. Enter the option number or
   its value, such as `fix` or `both`.
3. The app saves `pmm_manifest.json` in that folder. Commit it with your JTMods
   and images to your GitHub repository's default branch.

The console shows progress and stays open until you press Enter when launched
by double-clicking. Type `:quit` at any question to cancel without replacing the
manifest. The generator never uploads anything or changes a JTMod.

When run from an existing terminal, it uses the terminal's current directory
unless you pass a folder:

```powershell
.\PMMManifestGenerator.exe "C:\MyModsRepository"
```

The selected folder is the **repository root**: all manifest paths, including
images, are relative to it. Use `--recursive` to discover new JTMods in subfolders:

```powershell
.\PMMManifestGenerator.exe "C:\MyModsRepository" --recursive
```

Existing manifest entries in subfolders are checked even without `--recursive`.
The recursive scan skips `.git` and does not follow directory symlinks. Keep
backup/export copies of mods outside the scanned folders if you do not want to
publish them as separate entries.

## Images

Choose no image, an existing repository-relative image path (for example
`images/library-skybox-fix.png`), or extract the embedded PNG thumbnail when
the JTMod has one. Extracted thumbnails are saved after all questions have
been answered. The tool will not overwrite a different existing image.

Use PNG/JPEG for best compatibility with Mod Manager, at most **2 MiB** and
**4096 × 4096 pixels**. Image paths must remain inside the repository and must
refer to files you also commit. Windows backslashes entered at the prompt are
converted to `/`.

## Updating an existing manifest

Run the app again after exporting new or changed JTMods. It hashes the actual
files, so changes are detected even if their size and timestamp stay the same.

- Unchanged entries retain their values and order, including custom fields and
  a manually written catalog description. If nothing changed, the manifest's
  bytes and modification time are left untouched.
- Changed packages refresh their embedded metadata, size and hash. The existing
  **ID, Mod/Fix type, compatibility, and image path** are retained without asking
  again. A valid embedded description replaces the old description on updates;
  an empty embedded description keeps a valid existing catalog description.
- New JTMods are appended. An unambiguous rename with identical content retains
  the original ID automatically.
- If a new filename replaces a missing old file with different contents, the
  app asks whether it is a new mod or an update of the old entry. Selecting the
  old entry preserves its ID so existing subscribers can upgrade.
- For a missing JTMod, choose **Keep**, **Remove**, or **Cancel**. Keep is useful
  when a file already exists on GitHub but is absent from your local checkout.
- An existing ID cannot be moved to a different game.
- Invalid or corrupt packages stop the save; the existing manifest is retained.
  The manifest is replaced atomically only after all entries pass validation.

Use `--edit` to revisit classification, compatibility, and image choices for
existing entries even if their JTMods have not changed. IDs remain stable.

```powershell
.\PMMManifestGenerator.exe "C:\MyModsRepository" --edit
```

The generator detects Git LFS pointer files and tells you to run `git lfs pull`
first. It needs the actual JTMod bytes to read metadata and calculate the size
and checksum expected by Mod Manager. Downloading from GitHub is handled by Mod
Manager, not this offline authoring tool.

Other options: `--help` shows usage; `--no-pause` disables the final Enter prompt.
Exit code `0` means success (including no changes); `1` means an error or cancelled
input. Required choices have no assumed compatibility or Mod/Fix default.
