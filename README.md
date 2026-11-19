# GTA.js — Links & Resources

A curated archive of official DMA Design technical specifications, community documentation, prior engine reimplementations, classic modding tools, and reference data for **Grand Theft Auto (1997)**, collected while building [GTA.js](https://gtajs.wtf/).

Archived local mirrors of key specs, tutorials, and guides live inside [`./data/`](./data/).

## Contents

- [Official DMA Design Documentation](#official-dma-design-documentation)
- [Technical Specifications & Community Handbooks](#technical-specifications--community-handbooks)
- [`MISSION.INI` Tutorials & Scripting Reference](#missionini-tutorials--scripting-reference)
- [Engines, Clones & Reimplementations](#engines-clones--reimplementations)
- [Tools](#tools)
- [Original Game Data, Browser Play & Footage](#original-game-data-browser-play--footage)
- [Repository Layout](#repository-layout)

---

## Official DMA Design Documentation

Primary technical specs originally published by DMA Design:

| Document | Description | Local Copies | Source |
| --- | --- | --- | --- |
| **Mission Template Description** (`mtd.doc`) | Authoritative `MISSION.INI` scripting and mission-making guide | [`mtd.doc.md`](./data/mtd.doc.md) · [`mtd.pdf`](./data/mtd.pdf) · [`mtd.doc`](./data/mtd.doc) | [Wayback Archive](https://web.archive.org/*/https://gtamp.com/dl/file.php?id=52) |
| **CityScape Data Structure** (`cds.doc`) | Technical specification of the GTA 1 map (`.cmp`), style (`.g24`/`.g08`), and car handling formats | [`cds.doc.md`](./data/cds.doc.md) · [`cds.pdf`](./data/cds.pdf) · [`cds.doc`](./data/cds.doc) | [Wayback Archive](https://web.archive.org/*/https://gtamp.com/dl/file.php?id=51) |

---

## Technical Specifications & Community Handbooks

Deep-dive analyses, errata, and community references covering file formats, physics, and game mechanics:

| Resource | Description | Local Mirror | Source |
| --- | --- | --- | --- |
| **The Unofficial GTA Reference Handbook** | Comprehensive reference covering `.CMP`, `.GRY`/`.G24`, `.RAW`, `MISSION.INI`, objects, cars, and keywords (Michael Mendelsohn) | [`data/reference/`](./data/reference/) | [Wayback Archive](https://web.archive.org/web/20050219185438/http://gta.mendelsohn.de/Reference/) |
| **GTA 1 & London Technical Analysis and Patches** | File format specs (`.FON`, `.FXT`, `.SDT`, `.WAV`, savegames), `MISSION.INI` notes, and patches by Daniel Marschall | [`data/daniel-marschall/`](./data/daniel-marschall/) | [misc.daniel-marschall.de](https://misc.daniel-marschall.de/spiele/gta1/) |
| **Gouranga! Missions & Guides** | Classic mission scripting guides (*How to Write a Mission in 7 Days* and command specifics) | [`7days-guide.md`](./data/gouranga-missions/7days-guide.md) · [`specifics.md`](./data/gouranga-missions/specifics.md) | [Wayback Archive](https://web.archive.org/web/20050407123226/http://missions.gouranga.com/index.shtml) |
| **Notes on `cds.doc` & Car Handling** | Corrections and extra notes regarding `cds.doc` and vehicle data | [`data/notes.html`](./data/notes.html) | [Wayback Archive](https://web.archive.org/web/20080820104252/http://www.fifengr.com/gtacars/topic.html) |
| **2D Vehicles** *(Pat Kerr)* | JavaScript recreation of the 2D rigid-body physics simulation that became the basis of GTA 1's vehicle system, by its original author | — | [patkerr.co.uk/2d-vehicles](https://patkerr.co.uk/2d-vehicles/) |
| **gta.rdy.jp GTA 1 Database** *(Japanese)* | Detailed car database, wanted level mechanics, and game internals | [`data/gta-rdy-jp/gta1/`](./data/gta-rdy-jp/gta1/) | [gta.rdy.jp/gta1](https://gta.rdy.jp/gta1/) |
| **RAW & RAT File Format Description** | Structure of `.RAW` and `.RAT` files | — | [Wayback Archive](https://web.archive.org/web/20000622011740/http://www.multimania.com/dolmen/gta/raw.html) |
| **NPC Dialog Index** | Index of NPC dialog lines from Gouranga! | [`data/dialog.txt`](./data/dialog.txt) | [Wayback Archive](https://web.archive.org/web/20001205131800/http://www.gouranga.com/nf-info-gta.htm) |
| **GTA Tanks** | Details about tanks in the game | [`data/gtatanks.html`](./data/gtatanks.html) | [Wayback Archive](https://web.archive.org/web/20010307124036fw_/http://www.garson.clara.net/gta_tanks/gtatanks.html) |

---

## `MISSION.INI` Tutorials & Scripting Reference

### Project Cerbera Tutorials

| Topic | Local Mirror | Source |
| --- | --- | --- |
| Smallest `MISSION.INI` | [`data/smallest.md`](./data/smallest.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/smallest) |
| `SENDTO` command documentation | [`data/sendto.md`](./data/sendto.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/sendto) |
| Mission cleanup | [`data/mission-cleanup.md`](./data/mission-cleanup.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/mission-cleanup) |
| Insane stunts | [`data/insane-stunts.md`](./data/insane-stunts.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/insane-stunts) |
| Comments | [`data/comments.md`](./data/comments.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/comments) |
| `PARK` command example | [`data/park.md`](./data/park.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/park) |
| Characters in GTA 1 (found in `english.fxt`) | [`data/characters.md`](./data/characters.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/characters) |
| Random Pager Messages in GTA 1 coding | [`data/random-pager.md`](./data/random-pager.md) | [projectcerbera.com](https://projectcerbera.com/gta/1/tutorials/random-pager) |

### Reference Text Files

| File | Description |
| --- | --- |
| [`data/reference-block.txt`](./data/reference-block.txt) | Manipulating map block data |
| [`data/reference-car.txt`](./data/reference-car.txt) | Complete list of vehicle identifiers and relevant commands |
| [`data/reference-object.txt`](./data/reference-object.txt) | Using dynamic objects, such as potted trees and bollards |
| [`data/reference-ped.txt`](./data/reference-ped.txt) | Pedestrian remaps, AI types, and relevant commands |
| [`data/reference-powerup.txt`](./data/reference-powerup.txt) | Complete list of all valid powerups |
| [`data/reference-misc.txt`](./data/reference-misc.txt) | Two sets of uncommented `MISSION.INI` code examples |
| [`data/reference-misc2.txt`](./data/reference-misc2.txt) | Research notes and examples (crane `GTA_DEMAND`/`DO_GTA`, `KICKSTART` shortcuts, etc.) |

---

## Engines, Clones & Reimplementations

Open-source GTA 1 engines and map renderers useful as cross-references:

| Project | Stack | Notes |
| --- | --- | --- |
| [**niklasvh/WebGL-GTA**](https://github.com/niklasvh/WebGL-GTA) | WebGL / JS | Original WebGL map renderer |
| [**GiovanniNery/WebGL-GTA**](https://github.com/GiovanniNery/WebGL-GTA) | WebGL / JS | Continuation of the original `WebGL-GTA`; adds cars and extras, though buggy |
| [**VBGAMER45/gta1-webedition**](https://github.com/VBGAMER45/gta1-webedition) | WebGL / TS | Supports multiple maps, with render bugs and unconventional controls |
| [**madebr/OpenGTA**](https://github.com/madebr/OpenGTA) | C++ | Open-source GTA 1 engine reimplementation |
| [**codenamecpp/carnage3d**](https://github.com/codenamecpp/carnage3d/) | C++ | C++ GTA 1 engine reimplementation |
| [**angree/AmiGTA1**](https://github.com/angree/AmiGTA1/) | Amiga | Amiga reimplementation |

---

## Tools

### Open Source

- [**gta1-mapeditor**](https://github.com/VBGAMER45/gta1-mapeditor) — GTA 1 map editor.
- [**gta-g24-fxt-extractors**](https://github.com/VBGAMER45/gta-g24-fxt-extractors) — Python extractors for `.g24` and `.fxt` files.

### Classic / Closed Source

- [**FXT2TXT**](https://gta.mendelsohn.de/gman/fxt2txt/) — Decrypts and converts `.FXT` language files.
- [**Cops**](https://libertycity.net/files/gta-1/145066-cops-editing-police-paths.html) — Edits police patrol routes and cop map data.
- [**GTACars**](https://libertycity.net/files/gta-1/137805-gtacars-v.3.1.0s-and-v.3.2.3.html) — Classic vehicle handling and stats editor.
- [**Junction 25**](https://libertycity.ru/files/gta-1/138342-junction25-v.-1.6-i-v.-2.0beta.html) & [**M1**](https://libertycity.net/files/gta-1/138824-m1win-v0.95b.html) — Classic GTA 1 map editors.

---

## Original Game Data, Browser Play & Footage

### Game Data Archives

- **Full Game (1997)**:
  - [Internet Archive — Grand Theft Auto (1997, Rockstar Games)](https://archive.org/details/grandtheftauto1997rockstargames)
  - [Internet Archive — Grand Theft Auto (1997, DMA Design) `.zip`](https://archive.org/download/grand-theft-auto-1997-dma-design/Grand%20Theft%20Auto%20%281997%29%28DMA%20Design%29.zip)
  - Full-game `MISSION_INI.TXT`: [misc.daniel-marschall.de](https://misc.daniel-marschall.de/spiele/gta1/files/GTADATA/MISSION_INI.TXT) · Local copy: [`data/daniel-marschall/files/GTADATA/MISSION_INI.TXT`](./data/daniel-marschall/files/GTADATA/MISSION_INI.TXT)
- **1997 Shareware Demo**:
  - [Internet Archive — Grand Theft Auto Demo](https://archive.org/details/GrandTheftAutoDemo)

### Play the Original in DOSBox

- [Internet Archive — Grand Theft Auto (1997)](https://archive.org/details/grand-theft-auto-1997-dma-design) *(click the screen on the page to start)*
- [PlayDOSGames — Grand Theft Auto](https://www.playdosgames.com/play/grand-theft-auto/) *(the demo, with a time limit)*

### Original Gameplay Footage

- [General Gameplay (YouTube)](https://www.youtube.com/watch?v=Akmg6dUkwKc)
- [Kill Frenzy Missions (YouTube)](https://www.youtube.com/watch?v=Cn2eigBcV_0)

---

## Repository Layout

```text
├── README.md        # Index of documentation, tools, engines & game data links
├── data/            # Archived local copies of specs, tutorials, handbooks & reference files
└── repositories/    # Gitignored folder for optional local clones of open-source GTA 1 engines/tools
```