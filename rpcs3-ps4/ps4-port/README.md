# RPCS3 PORT FOR PS4 - by Roberix Studio & Mifi Studios

Port of [RPCS3](https://rpcs3.net), the PlayStation 3 emulator, for consoles
PS4 unlocked with GoldHEN. The emulation core is the original code of the
RPCS3, compiled for the PS4 with the OpenOrbis toolchain; the port adds the
console frontend, the native adapters (memory, video, audio, controls,
network, USB) and a XMB-style launcher.

Current version: **0.41** (TITLE_ID `RPCS30010`).

Unofficial project, with no connection with Sony or the RPCS3 team.
Package does not include games, firmware, keys or licenses: use copies only
of your own games.

## Features

- **LLVM recompilers of PPU and SPU** of RPCS3 running on the PS4 itself
  (LLVM fixed by the RPCS3 submodule), in addition to the SPU ASMJIT and the
  interpreters.
- **Vulkan** by Mesa RADV driver (original `VKGSRender` renderer).
- **OpenGL** by Mesa EGL + Zink over RADV (`GLGSRender` renderer
  original).
- **Automatic configuration**: when opening a game, the indicated profile is applied
  for PS4 and known fixes for each title. If a game fails, the
  next boot adjusts itself (fewer compilation threads, fewer
  VRAM). Own configurations per game remain possible.
- **Up to 4 players**, one DualShock 4 controller per user logged in to the PS4, with
  the color of each player's light bar.
- **Online via RPCN** (RPCS3's own server): import from `rpcn.yml`
  RPCS3 PC, login and account creation via on-screen keyboard, UPnP
  optional.
- **XMB Launcher**: game list (folders, ISO, PKG), installation of PKG from
  PS3 (games, updates, DLC), import of `.rap` licenses, settings
  global and per-game system information. Use fonts and icons from
  PS3 firmware copied to console when available.
- **PS3 disc on PS4 player**: the disc is read by the FTP server on the PS4
  GoldHEN itself (read only) and installed once in `/data/rpcs3/games`;
  the disc key is fetched by itself (IRD, like PC's PS3 Disc Dumper) and
  the copy is already decrypted. Circle cancels and installation continues afterwards
  where it left off.
- Return the game to XMB without closing the application (L1 + R1 + Triangle per
  1 second, or L1 + R1 + Options for 2 seconds).
- **Game patches** like on PC: import (FTP or pendrive), download
  rpcs3.net via HTTPS verified and list per game (R1).
- **Cheats** (PC codes Artemis/NetCheat and `cheats.yml` from RPCS3): R2
  about a game on XMB, or the PS button menu within the game.
- **Languages**: XMB, windows, warnings and settings in Portuguese (Brazil),
  English, Español and العربية (Arabic, from right to left). Follow the
  PS4 language, or choose Settings > "Language · Language · اللغة".
- **Complete RPCS3 Settings**: PC RPCS3 tabs (CPU, GPU,
  Audio, E/S, System, Network, Advanced, Emulator, Debug) and the PS4 tab; each
  option shows the value that will run and who decides. The file for each game
  Keep only what you changed.
- Diagnosis: records of the last sessions (`startup.log`, `RPCS3.log`,
  `mesa.log`) and crash report on screen.

## What's new in 0.40

- **New rule**: what 0.39 changed for all games without proof in the logs
  back to 0.38 way (Mesa shader cache, "Continuous audio"
  automatic and 64 ms reservation, new attempts to create threads, rules
  memory and compilation, game language and others). Each league of
  new with a file in `/data/rpcs3` or by `features.txt`, to test.
  Proven fixes (such as the crash of the
  shaders) and protections that only act when something goes wrong.
- **Safe Mode**: A game that crashes opens next time in safe mode (3
  levels, with on-screen warning). You can also choose by game (Square >
  "Safe mode for this game") or turn on all games in Game Settings.
  RPCS3 > PS4 > "Safe Mode".
- **Threads**: the last 8 PS4 thread slots are for the game; the
  service threads that did nothing are no longer created; the pause and the
  savestate no longer crashes the emulator when thread is missing; the sound doesn't
  it should disappear after a pause close to the limit.
- **Online and language**: Online mode without RPCN account opens the game offline; the
  "Automatic" of "Game Language" has returned to English (US), and the language
  of the PS4 became an option in the line.
- **Graphics**: the shader interpreter has a step limit, the GPU
  locked back to XMB and a skipped drawing never counts as
  "hidden" (Sonic 06).
- **Settings**: each option runs as you chose; just a real limit
  of the PS4 wins your choice, with the padlock and the reason on the page (e.g.:
  "Resolution scale" up to 200 %). `startup.log` tells you which values run
  different from your files and why.
- **Discs and ISOs without internet**: the key can come from a file made from
  your own disk (`.dkey`, `.ird` or key list) in
  `/data/rpcs3/keys` or in the `keys` folder on the pendrive, and each key that opened
  a disc is stored on the PS4. The package remains without keys. To read the
  reader, the PS4 still needs to be on a network (the FTP of the GoldHEN).
- **XMB**: the background and music of the game itself when the selection stops there
  (only the game files; the package does not include images or music), folders in the
  "Game" column (Triangle = Options) and the second keyboard in a row (RPCN,
  cheats) opens.
- **New icon** for the application.
- The first boot of each game in 0.40 mounts the SPU cache again: no
  close.

Almost all of this still needs to be tested on the console: the roadmap is in
"TEST ON 0.40 CONSOLE", in [`TEST_ON_PS4.txt`](TEST_ON_PS4.txt)
(send the logs of each test).

## What's new in 0.39

- **Crashes and freezes (all games)**: interpreter crash fixed
  of shaders after a few minutes of gameplay; the emulated GPU reads and writes the
  sync tags without going through memory protection (the cause
  likely to freeze GTA IV); When the GPU stops responding, the game
  closes and returns to XMB (before it was stopped).
- **Threads**: threads that have finished are released, a creation refused by the
  PS4 is retried and service threads only start when the game uses
  (games that were muted or returned to the menu).
- **Audio**: Automatic "continuous audio" (the sound is a little slower,
  never below 70%, instead of chopping) and 64 ms reserve; the adjustments of
  audio from the game menu are immediately valid.
- **Discs and ISOs**: a ISO never asks for the disc on the reader (only the image that the
  line "PS3 Disc" installed); ISO encrypted opens without manual key (the
  key is fetched alone); Incomplete ISO is notified in the list; ISO without the
  PS3 disc header opens; the disc taken during the game pauses the game.
- **FTP**: folders created by FTP from GoldHEN are locked for the
  emulator; with FTP turned on, RPCS3 fixes this by itself when opening.
- **Video**: Mesa driver shader cache turned on by default (the second
  opening a game compiles much less) and OpenGL lighter.
- **New**: XMB languages, full RPCS3 Settings and
  cheats (see Resources); RPCS3 PC game settings
  (`config_database.dat`) come inside the package.

Details, previous versions, and console testing roadmap are at
[`TEST_ON_PS4.txt`](TEST_ON_PS4.txt).

## Requirements

Console:

- PS4 or PS4 Pro unlocked with **GoldHEN** (on PS4 Pro, activate Mode
  Turbo/Boost Mode in system settings);
- official PS3 firmware (`PS3UPDAT.PUP`), installed by XMB (see below;
  it is in `/data/rpcs3/dev_flash`);
- your PS3 games (disc folder, ISO or PKG).

The PS4's Jaguar CPU is much weaker than that of a PC: light games run well,
Heavy games can have low FPS. The first opening of each game
compiles its code ("Compiling PPU Modules" / "Building SPU Cache") and can
take many minutes; the next ones use the cache.

## Installation

1. Copy `.pkg` to `/data/pkg` and install from GoldHEN
   (Debug Settings > Package Installer). Updating the package maintains the
   settings and caches.
2. With `PS3UPDAT.PUP` on the USB stick (`PS3/UPDATE/` or root), use XMB > Settings > Install PS3 Firmware.
3. Place the games in one of these folders:
   - `/data/rpcs3/games/NOME` (`PS3_GAME/USRDIR/EBOOT.BIN`) or ISO in
     `/data/rpcs3/games`;
   - PKG from PS3 in `/data/rpcs3/pkgs`;
   - on the pendrive: `PS3`, `GAMES`, `PS3ISO` or `PKG`;
   - `.rap` licenses in `/data/rpcs3/raps` (or next to PKG);
   - or place the PS3 disc in the PS4 player (with the FTP of the GoldHEN
     on) and choose "PS3 Disc" from the list to install it.
4. Open **RPCS3 PORT FOR PS4** from the home screen.

The complete usage guide (controls, menus, advanced options, registrations) is at
[`TUTORIAL.txt`](TUTORIAL.txt) and [`TEST_ON_PS4.txt`](TEST_ON_PS4.txt), which are also included in the package.

## How to compile

The build runs on a Windows host and generates the native ELF, EBOOT, and PKG.

### Ferramentas

- [OpenOrbis PS4 Toolchain](https://github.com/OpenOrbis/OpenOrbis-PS4-Toolchain)
  (includes `create-fself`, `create-gp4` and `PkgTool.Core`, which requires .NET);
- [llvm-mingw](https://github.com/mstorsjo/llvm-mingw) 20240619 (UCRT,
  x86_64), used as a cross compiler (Clang/LLD) and for host testing;
- CMake 3.28 or newer and Ninja (`build-llvm.ps1` uses
  `ps4-port/.build-tools/ninja.exe`);
- Python 3.12 or newer, Git and Git for Windows (its bash compiles
  FFmpeg);
- Windows PowerShell 5.1 or PowerShell 7.

### Source code

This repository (with the RPCS3 submodules) and, in the same parent folder:

```
<pasta>/
  rpcs3-ps4/            este repositório
  orbis-compat/         runtime de compatibilidade POSIX/libkernel
  orbis-porting-kit/    carregador Vulkan (vkloader)
  orbis-mesa-bundle/orbis-mesa-4c934ebf9cda/   build do Mesa (RADV, EGL, Gallium/Zink)
```

Paths can be changed with `RPCS3_PS4_ORBIS_COMPAT`,
`RPCS3_PS4_PORTING_KIT` and `RPCS3_PS4_MESA_BUNDLE`.

### Passos

In the root of the repository, under PowerShell (set `$Sdk` and `$Llvm` to your
environment; the scripts accept `-Sdk` and `-LlvmTools`/KEEP4TOKEN, and the
default values point to folders neighboring the checkout):

```powershell
$Sdk  = '<caminho>/OpenOrbis/PS4Toolchain'
$Llvm = '<caminho>/llvm-mingw-20240619-ucrt-x86_64/bin'

git submodule update --init --recursive

# Fixed dependencies: Abseil, GNU Make (MSYS) and LLVM for the console
./ps4-port/prepare-abseil.ps1
./ps4-port/prepare-make.ps1
./ps4-port/build-llvm.ps1 -Sdk $Sdk -CompilerBin $Llvm -Jobs 4

# Configuration (build folder must be ps4-port/.build-abi)
cmake -S . -B ps4-port/.build-abi -G Ninja `
  -DRPCS3_PS4_PORT=ON `
  -DCMAKE_TOOLCHAIN_FILE=ps4-port/toolchain.cmake `
  "-DRPCS3_PS4_SDK=$Sdk" "-DRPCS3_PS4_LLVM_BIN=$Llvm" `
  -DRPCS3_PS4_WITH_LLVM=ON

# Native ELF (FFmpeg and curl are automatically compiled)
cmake --build ps4-port/.build-abi --target rpcs3_ps4_elf -j 4

# EBOOT + PKG validados
./ps4-port/package-native.ps1 -Sdk $Sdk -LlvmTools $Llvm
```

`package-native.ps1` checks the version in `rpcs3/util/ps4_build_version.hpp`,
validates the ELF (`verify-native-dispatch.py` and the exit policy of the
process), mounts the PKG, extracts the contents back and compares the hashes of
each file. The result is in `ps4-port/dist/`:
`RPCS3_PS4_NATIVE_<versão>_<data>.pkg`, `.sha256`, `build-manifest` and
the `TUTORIAL.txt` and `TEST_ON_PS4.txt` guides.

Optional: `-ConfigDatabase <caminho>/GuiConfigs/config_database.dat` places the
bank of RPCS3 PC tweaks per game in the package, such as
`/app0/config_database.dat` (the `config_database` field of `build-manifest`
shows his `timestamp`, or `none`). Comes unplugged: use only after
confirm that the file license allows you to distribute it
(`rpcs3/util/ps4_config_db.hpp`).

Patch download root certificates are
`ps4-port/assets/certs/cacert.pem` (copy of
`3rdparty/wolfssl/wolfssl/wolfcrypt/src/port/Espressif/esp_crt_bundle/cacrt_all.pem`:
148 Mozilla roots, data from 2024-07-02), installed on
`/app0/certs/cacert.pem`.

### Testes de host

`ps4-port/tests/` contains tests that run on PC (Windows) with calls from the
Simulated PS4 kernel: virtual and shared memory, memory heap
direct from orbis-compat, fenv, atomic events, configuration policy,
control vacancies, RPCN, XMB, USB/MIDI account, audio and others. The line of
compilation of each test is in the file header.

## Project structure

| Path | Content |
| --- | --- |
| `ps4-port/CMakeLists.txt`, `*.cmake` | cross build: toolchain, dependencies, LLVM, native link |
| `ps4-port/*.ps1`, `ps4-port/*.py` | dependency preparation, LLVM and FFmpeg build, ELF verification and packaging |
| `ps4-port/runtime/` | PS4 implementations: virtual and shared memory, fenv, `exit()` guard, USB (sceUsbd), MIDI, audio capture, wolfSSL seed |
| `ps4-port/compat/` | OpenOrbis SDK compatibility headers |
| `ps4-port/tests/` | host tests and their mocks |
| `ps4-port/tools/` | support tools (profiler symbols, string search in binaries) |
| `ps4-port/graphics/` | notes on presentation Vulkan/OpenGL |
| `ps4-port/probe/` | minimal program to test the toolchain in the console |
| `ps4-port/assets/` | application icon and root certificates (`certs/cacert.pem`) |
| `rpcs3/ps4_*.cpp` | console frontend: input, XMB launcher, controls, on-screen keyboard, fatal error, profiler |
| `rpcs3/util/ps4_*.hpp` | configuration policy, memory, XMB, RPCN, system health and port utilities |
| `rpcs3/Emu/Audio/PS4/` | audio backend (AudioOut) |
| `rpcs3/Emu/NP/ps4_rpcn_service.*` | console service RPCN |
| `rpcs3/Emu/NP/ps4_tls_selftest.*` | TLS test from scratch (libcurl + wolfSSL via loopback, without internet) |
| `ps4-port/curl.cmake`, `ps4-port/compat/ps4_curl_socket.h` | libcurl build for PS4 (without `eventfd`, sockets not blocked by `SO_NBIO`, without IPv6) |
| `rpcs3/Input/ps4_pad_handler.*` | DualShock 4 controls (libScePad) |

## Credits and licenses

**RPCS3** - all emulation core, renderers, recompilers and
interface icons are from the RPCS3 project, the RPCS3 team and their
collaborators (https://rpcs3.net, https://github.com/RPCS3/rpcs3).
License **GPL-2.0** (`LICENSE` in root, also included in PKG). This port is
distributed under the same license.

Port for PS4: **Roberix Studio & Mifi Studios**.

Third-party components used in the console build:

| Component | Usage | License |
| --- | --- | --- |
| [LLVM](https://llvm.org) | recompilers PPU/SPU | Apache-2.0 WITH LLVM-exception (`licenses/LLVM.txt` no PKG) |
| [Mesa](https://mesa3d.org) (RADV, EGL, Gallium/Zink) | Vulkan and OpenGL | MIT |
| [FFmpeg](https://ffmpeg.org) n8.1.1 | audio decoding/video | LGPL-2.1-or-later (compiled without `--enable-gpl`/KEEP26TOKEN) |
| [wolfSSL](https://www.wolfssl.com) | TLS (RPCN, curl) | GPL-3.0, used as GPL-2.0 by the wolfSSL exception for RPCS3 |
| [curl](https://curl.se) | HTTP | curl license (MIT style) |
| Mozilla Root Certificates (`certs/cacert.pem`, from wolfSSL `cacrt_all.pem`) | HTTPS verification of patch download | MPL-2.0 |
| [miniupnpc](https://miniupnp.tuxfamily.org) | UPnP | BSD-3-Clause |
| [Protocol Buffers](https://protobuf.dev) | RPCN protocol | BSD-3-Clause |
| [Abseil](https://abseil.io) | Protobuf dependency | Apache-2.0 |
| [zlib](https://zlib.net) | compression (from the Mesa package) | Zlib |
| [zstd](https://github.com/facebook/zstd) | compression | BSD-3-Clause (or GPL-2.0) |
| [libpng](http://www.libpng.org) | PNG | libpng-2.0 |
| [AsmJit](https://asmjit.com) | SPU ASMJIT | Zlib |
| [yaml-cpp](https://github.com/jbeder/yaml-cpp) | settings | MIT |
| [pugixml](https://pugixml.org) | XML | MIT |
| [SoundTouch](https://www.surina.net/soundtouch) | audio | LGPL-2.1 |
| [Fusion](https://github.com/xioTechnologies/Fusion) | motion sensors | MIT |
| [glslang](https://github.com/KhronosGroup/glslang) | SPIR-V | BSD-3-Clause and others (see glslang's `LICENSE.txt`) |
| [Vulkan Memory Allocator](https://gpuopen.com/vulkan-memory-allocator) | Vulkan | MIT |
| Headers Vulkan (Khronos) | Vulkan | Apache-2.0 OR MIT |
| [stb](https://github.com/nothings/stb) | images and fonts | MIT or public domain |
| [libusb](https://libusb.info) (headers) | USB API implemented on top of sceUsbd | LGPL-2.1 |
| [RtMidi](https://github.com/thestk/rtmidi) (API C) | MIDI USB | RtMidi license (MIT style) |
| [OpenOrbis PS4 Toolchain](https://github.com/OpenOrbis/OpenOrbis-PS4-Toolchain) | SDK and packaging tools; musl runtime (MIT) and libc++/libc++abi/libunwind (Apache-2.0 WITH LLVM-exception) | GPL-3.0 (tools) |
| orbis-compat | compatibility runtime POSIX/libkernel | MIT (exceptions indicated by file) |
| [orbis-porting-kit](https://github.com/orbis-ports/orbis-porting-kit) | Vulkan loader (vkloader) | MIT (exceptions indicated by file) |
| [DejaVu Fonts](https://dejavu-fonts.github.io) | launcher fonts | Bitstream Vera / DejaVu license (changes in public domain) |

The complete licenses are in the directories for each component in `3rdparty/`
and in the aforementioned repositories. PlayStation, PS3 and PS4 are Sony brands
Interactive Entertainment.
