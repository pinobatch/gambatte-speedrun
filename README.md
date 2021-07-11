# Gambatte-Speedrun (Non-PSR)

Fork of [Gambatte](https://github.com/sinamas/gambatte) (authored by sinamas), with local changes for Pokémon Speedruns, as well as other speedrunning communities. Under GPLv2.

Below is a brief list of the major differences between Gambatte-Speedrun and the upstream version of Gambatte.

---
***Emulation core***

* Original Game Boy games playable in Game Boy Color mode (emulated properly)
* Super Game Boy emulation (to match the SGB2 platform/framerate)
* Cycle-based RTC option (for *"proper"* RTC behavior when using speedups/pauses/savestates)
* Other various emulation fixes, improvements, and configuration options

***Speedrun features***

* Timing parity with official consoles (bootroms required; GBP/SGB2 hard reset fades emulated)
* Emulator version and ROM info displayed in the title bar, and in on-screen display after hard resets
* Disabling of undesired functionality for speedruns (e.g. cheats, turbo, framerate changes, etc.)

***Software updates***

* Updated to build using Qt5 (instead of Qt4)
* Buildable as a shared library, for programmatic use of the emulation core

---
## Building from source

Distributable binaries can be built on Windows using [MSYS2](https://www.msys2.org/) and the qt5-static package, or on macOS using [Homebrew](https://brew.sh/) and the qt package with its `macdeployqt` tool. See [INSTALL.md](INSTALL.md) for detailed information on how to set up the build environment.

The amount of setup you need to do depends on what parts of the project you are planning to use.

### Shared Library

If you only wish to build Gambatte-Speedrun's `libgambatte` shared library (for use in scripting, "botting", TASing, or other programming projects), and have the basic build environment set up, you can run the following from the project's root directory, regardless of platform:
```
$ sh scripts/build_shlib.sh
```

### Gambatte-Speedrun *(i.e. the full-blown emulator)*

After completing all the Qt-specific build steps in [INSTALL.md](INSTALL.md), running the following in the project's root directory should build the "PSR" version of Gambatte-Speedrun:
```
$ sh scripts/build_qt.sh
```
**This fork builds the "non-PSR" version by default,** with additional selectable platforms (GB, GBC, GBA, SGB2). This is controlled by creating `gambatte_qt/src/platforms.pri` with the following (before running `build_qt.sh`):
```
# platform support
# GBP is hardcoded
DEFINES += SHOW_PLATFORM_GB
DEFINES += SHOW_PLATFORM_GBC
DEFINES += SHOW_PLATFORM_GBA
DEFINES += SHOW_PLATFORM_SGB
```
To build the "PSR" version, which supports only the Game Boy Player, remove the file `gambatte_qt/src/platforms.pri`.

**Boot ROM verification is disabled in this fork.**  Unlike in PSR's Gambatte-Speedrun, the boot ROMs are not required to match Nintendo's copyrighted boot ROMs bit for bit.  Instead, a free replacement boot ROM such as SameBoot from [SameBoy](https://sameboy.github.io/) may be used, at the possible cost of desynchronizing the random number generator in _Pokémon_ and other games that rely on the `DIV` register.  If you use a replacement boot ROM, make sure to change Settings > Platform to either "Game Boy" or "Game Boy Color", as Gambatte-Speedrun applies patches to the boot ROM in Game Boy Advance or Game Boy Player mode that cause emulation to freeze when applied to SameBoot. (Thanks to CasualPokePlayer in gbdev Discord for helping to troubleshoot this.)

### Testrunner

To be able to run the upstream hwtests suite on Gambatte-Speedrun, you must acquire the DMG and CGB bootroms. Name the DMG bootrom `bios.gb`, the CGB bootrom `bios.gbc`, and move both into the `test` directory.

Run the following in a terminal from the project's root directory to assemble and run all hwtests:
```
$ (cd test && sh scripts/assemble_tests.sh)
$ sh scripts/test.sh
```
Note that the first line (with `assemble_tests.sh`) only needs to be run one time, or until the contents of the hwtests directory change.