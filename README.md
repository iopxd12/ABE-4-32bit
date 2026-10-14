# Angry Birds Epic 1.4.2 — Offline

This version of **Angry Birds Epic 1.4.2** for iOS runs with **no server and no internet**. The game was delisted and Rovio's servers are shut down; this build lets it start and play entirely on the device.

> Angry Birds Epic was made by **Rovio Entertainment**, who own all of its code, art, music and text. This project only changes a few bytes of the game so it runs offline. It is not affiliated with or endorsed by Rovio.

## Status

| | |
|---|---|
| **Build** | Offline r5 (`gold_offline`, md5 `e6b38ae5…`) |
| **Tested on** | iPad 4th gen, iOS 8.4.1 (jailbroken) |
| **Verified** | Fresh start with no save → new player created → world map |

## Requirements

- An iPhone or iPad with **32-bit (armv7) support** running **iOS 7 or later**. 1.4.2 is the last version whose IPA contains 32-bit code.
- A way to sideload: Sideloadly, AltStore, TrollStore, or AppSync on a jailbroken device.

## Install

1. Sideload `AngryBirdsEpic_1.4.2_Offline.ipa`.
2. Open the game.
3. To confirm the version, go to Options → Credits; the label ends in **"- Offline"**. The map loading screen also shows "Offline" in its top-right corner.

## What was changed

All changes are small byte patches to the game's own code, plus one text table entry and one loading-screen mark. Nothing new is added to the app.

- **No server:**
  - The network check always says "online".
  - Login, cloud save, inbox and downloads use the stand-in services Rovio already shipped inside the game.
  - Startup skips the login, version check and download steps.
- **Built-in data:** game data, game text and event data always load from the copies inside the app. On iOS 8, reinstalling moves the app's folder, which left stale paths to old downloads and hung the loading screens.
- **New players:** with no save, a fresh profile is created on the device. Rovio's offline cloud-save stand-in never answers, so this used to hang on "Loading Balancing into memory".
- **Shop:** purchases use the game's built-in stand-in store. Prices show "FREE" and no real payment can happen.
- **Bug fix:** the world map no longer breaks when a shop offer icon can't load. Before, its buttons vanished and a Special Offer sat mid-screen.

## Building

### From an original 1.4.2 IPA (public patcher)

```
python3 release/abe-offline/patch_offline.py "Angry Birds Epic 1.4.2.ipa" AngryBirdsEpic_1.4.2_Offline.ipa
```

- **Requirements:** Python 3.8+ only.
- **Safety checks:** the patcher refuses to run unless the input files are the original 1.4.2 files, and it verifies its output.
- **Loading screen:** the public build keeps the stock loading screen.

### From this project (full build)

```
python -I scripts/build_142_variant.py builds/1.4.2/gold_stock builds/1.4.2/gold_current_deployed builds/1.4.2/gold_offline offline
python -I scripts/make_ipa.py "Angry Birds Epic 1.4.2.ipa" gold_offline dist/AngryBirdsEpic_1.4.2_Offline.ipa
```

- The `offline` preset applies every Offline patch and checks the result's md5.
- `make_ipa.py` adds the metadata with the "- Offline" label and the marked loading screen (`builds/1.4.2/REQUIRES.tsv`).
- Earlier builds are kept as presets `offline_r1` … `offline_r4` for rollback.

## Testing on the iPad (USB)

```
.\scripts\device\abe.ps1 deploy gold_offline     # back up, upload, verify, ad-hoc sign (needed for Sideloadly installs)
.\scripts\device\abe.ps1 launch
.\scripts\device\abe.ps1 shot                    # screenshot
.\scripts\device\abe.ps1 stop                    # always close the game after a test
```

- **Where the save is:** `/var/mobile/Library/Preferences/com.rovio.gold.plist`. It survives deleting the app.
- **To test as a new player:** stop the game, back up and move that file aside, then run `killall cfprefsd`.

## Known limits

- **Single-player only.** No PvP or friends, no server events, news or inbox.
- **The save lives only on the device.** Back it up yourself if you care about it.
- **Shop icons:** some offer icons may be missing. The downloadable icon pack that survives was built for a newer Unity engine, which 1.4.2 can't load.

## Credits

- **Rovio Entertainment:** made Angry Birds Epic. Angry Birds Epic © Rovio Entertainment Corporation; Angry Birds and all related names, characters and art are trademarks of Rovio.
- **Angry Birds EPIC Central Discord:** the community keeping the game alive.
- **Claude (Anthropic):** reverse engineering, patches and tools.
