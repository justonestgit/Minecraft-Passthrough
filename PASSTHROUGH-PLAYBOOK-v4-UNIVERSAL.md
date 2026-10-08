# ENGLISH-ONLY AI AGENT EDITION

> All normative instructions, procedures, gates, terminology, and project documentation in this playbook are written in English for reliable use by AI coding agents.

# UNIVERSAL PASSTHROUGH PLAYBOOK — Game A ↔ Game B
## Consolidated Edition: Hardened Passthrough + AI Game Modding Guides

> Operational specification for genuine runtime passthroughs between two games. Game A and Game B
> are roles, not fixed titles. Minecraft is reference prior art, not a universal requirement.

## UNIVERSAL OVERRIDES

1. **Host/guest are dynamic.** Define HOST, GUEST, player authority, render authority and physics authority for the exact pair.
2. **Route before code.** Classify the idea as live state bridge, frame compositing, native geometry/collision, shared simulation, engine recreation, asset conversion, rebuilt mechanic, or a hybrid.
3. **Evidence before assumptions.** Exact installed build evidence outranks historical notes.
4. **Prior art first.** Search Universal Modder knowledge, community loaders, existing mods and close passthroughs before inventing hooks.
5. **Universal Modder is the technical execution layer.** Use @mashup-mods, @game-recon, @reverse-engineering and relevant skills/knowledge when available.
6. **The bridge contract comes before bridge code.** Record versions, ownership, axes, units, messages, frames, lifecycle and failure semantics.
7. **Unknown is not pass.** Critical UNKNOWN gates block irreversible implementation.
8. **The human is the final visual/feel oracle.** The agent should use telemetry and deterministic tests rather than pretending it can visually judge the game.
9. **Snapshots and events are different.** Latest-value state may be overwritten; one-shot events need bounded queues and explicit overflow semantics.
10. **Collision is bidirectional.** Host geometry does not automatically collide with guest objects, and guest geometry does not automatically block host actors.
11. **Version every boundary.** Protocols, generated files, loaders, game builds and inherited code need explicit versioning.
12. **Circuit-break repeated failure.** After about three equivalent failed attempts without new evidence, stop and reassess.
13. **No bypasses.** Never bypass anti-cheat, DRM, ownership checks or protected online systems.
14. **Publish provenance.** Record upstream projects, exact commits/releases, licenses, inherited/new work and AI/human contributions.

## UNIVERSAL VIABILITY GATES

V1 legitimate mod route for both games
V2 exact build/version pinned
V3 loader/API/hook surface
V4 reliable IPC
V5 camera/projection access
V6 viable render/depth/geometry route
V7 collision/physics/state access
V8 authority and hand-back model
V9 lifecycle/restart/recovery
V10 reproducible offline/controlled testing
V11 security/anti-cheat/DRM
V12 oracle coverage
V13 repository/licensing/lineage safety

Critical UNKNOWN remains blocking. BLOCKED safety gates terminate the route.

## ROUTE MATRIX

| Route | Strength | Main cost |
|---|---|---|
| Live passthrough | Real guest gameplay | Both sides and a broad state bridge |
| Frame compositing | Fast visual prototype | GPU→CPU→GPU transfer, weak native lighting/occlusion |
| Native geometry | Native host lighting/shadows | Hard render integration |
| Shared simulation | Consistent cross-game rules | Does not preserve every game's native physics |
| Engine recreation | Portability/control | Large reverse-engineering/reimplementation effort |
| Asset conversion | Smallest scope | Not the original runtime behaviour |
| Rebuilt mechanic | Focused scope | Only reproduces selected mechanics |
| Hybrid | Best practical compromise | More interfaces to test |

## PRIOR-ART / CASE-STUDY LESSONS

Use SkyCraft/FalloutCraft/OWCraft, Minecraft×GTA V, NewVegasCraft, CrossOver bridges,
Minecraft×Half-Life, GalaxyCraft, Signet, Faith Runner, AC1 Movement Rewritten, Skate-engine
integrations and Universal Modder examples as reference families. Reuse concepts and representations,
not unvalidated offsets or assumptions.

The public AI Game Modding Guides repository emphasizes: install first; search existing work first;
use an agent harness rather than browser chat for local files/builds; test a known-good loader;
start in a clean project; playtest repeatedly; keep notes and handoff files; never commit game files;
and publish exact versions, limitations, credits and AI disclosure. It also documents route selection,
loader families, reverse-engineering workflows, testing/troubleshooting, model/cost tradeoffs,
legal/provenance considerations and ownership/sync/rendering failure modes.

## PROJECT MEMORY FILES

Maintain:
- AGENTS.md
- MODLOG.md
- STATUS-handoff.md
- docs/DESIGN.md
- docs/BRIDGE-CONTRACT.md
- PLAYTEST-report.md
- THIRD-PARTY-NOTICES.md
- ATTRIBUTION-and-lineage.md
- README.md

## AGENT WORKFLOW

Plan → recon → viability report → bridge contract → minimal slice → build → real test → log →
commit → next slice. One feature at a time. Ask before major refactors. Say "not tested" when not
tested. Record failures as well as successes.

## MODEL/AGENT GUIDANCE

Use an agent harness capable of reading local game folders, editing/building projects and reading
logs. Model recommendations are deliberately not hard-coded because they change. A strong model can
plan/reason while a cheaper model implements repetitive slices; an independent reviewer can audit.
Fresh contexts plus STATUS-handoff files prevent long sessions from becoming noisy.

## LEGAL / PUBLISHING

Use only games you own and legitimate mod surfaces. Do not bypass DRM, anti-cheat or ownership
checks. Do not publish extracted assets, leaked code or decompiler databases. Keep a whitelist
.gitignore. Record licenses and upstream lineage. State exact versions, known limitations, AI use
and unofficial status. This is not legal advice.

## RESEARCH REFERENCES

AI Game Modding Guides: https://github.com/trevaintdead/ai-game-modding-guides
Universal Modder: https://github.com/rehan-remade/universal-modder

The source guide set includes start-here, agent setup, passthroughs, Rust rewrites/ports,
prompting/workflow, testing/troubleshooting, legal/publishing, FAQ, loaders/script extenders,
worked passthrough, project posting, models/costs, worked rewrite, reverse engineering/law,
route selection, case studies and ownership/sync/rendering, plus reusable AGENTS, STATUS, MODLOG,
workflow, bridge-contract, playtest and attribution templates.# ORIGINAL HARDENED PLAYBOOK CONTENT
---
# PLAYBOOK — Passthrough Minecraft ↔ Game X (Technically Compatible)

> **This document is an operational procedure, not an article.** You (the agent) will follow the phases in
> order, pass the gates, and end up with a working passthrough in the actual game.
>
> **Intended use:** the user invokes `@mashup-mods` with this file and asks for "a Minecraft passthrough
> with <game>". From that point on, everything here is executable.


## HARDENING LAYER — Agent Execution Rules (v2.0)

> This section is a governance layer for the entire playbook. It does not replace the phases below.
> It defines how the agent must reason, consult knowledge, state uncertainty, and decide whether it is worth
> continuing. The goal is to prevent a hypothesis from becoming fact and causing the agent to build on
> a false foundation.

### H0.1 — What This Document Promises

This playbook is **universal in method, not in outcome**.

The agent may attempt a passthrough for any technically compatible game, but **must not promise
compatibility in advance**. Compatibility exists only after the critical gates have been proven for the
**exact game version/build**.

Required outcome classifications:

- 🟢 **VERIFIED** — proven on the target build, with a reproducible oracle.
- 🟡 **PARTIAL** — part of the path is proven; there are blockers or components that have not yet been validated.
- 🟠 **PROTOTYPE** — functional demonstration, but without final robustness/performance/compatibility.
- 🔴 **BLOCKED** — a technical, security, or compatibility blocker prevents continuing.
- ⚪ **UNKNOWN** — there is not yet sufficient evidence. UNKNOWN **must not be treated as PASS**.

### H0.2 — HOST and GUEST Are Roles, Not Game Names

Before implementing anything, explicitly write:

HOST  = process that provides the final passthrough experience/rendering.
GUEST = process that provides the world, gameplay, state, or embedded content.

Minecraft can be HOST or GUEST.
Game X can be HOST or GUEST.

The choice must follow from the discovered capabilities, especially camera, framebuffer/depth,
geometry injection, and rendering cost. Never assume that Minecraft needs to be the HOST.

### H0.3 — Delegating Knowledge to Universal Modder

If universal-modder and/or @mashup-mods are available, they are a required part of the
runtime environment.

The playbook defines WHAT needs to be discovered, validated, and built.
The Universal Modder skills/, knowledges/, and tools define HOW to perform the operations
specific to each game/engine.

Before any game-specific implementation:

1. consult @mashup-mods;
2. use @reverse-engineering to inspect the requthisd game's files, determine whether the passthrough is possible, and identify possible adaptations based on the game's code;
3. search the Universal Modder knowledge base for the exact game;
4. search for the engine + version;
5. read the relevant skills/knowledges;
6. record which sources were used in MODLOG.md;
7. validate the instructions against the installed build.

**Do not invent a modding technique when relevant knowledge is available.**

If outdated knowledge contradicts evidence obtained from the current build, treat the knowledge as a
historical reference and follow the current evidence. Never copy offsets, signatures, addresses, class
names, structures, or versions from another build without revalidating them.

### H0.4 — Final Feasibility Gate Before Creating the Project

After Recon and before writing the first game-specific mod, all of these items must be
classified:

| Gate | Question | Permitted Result |
|---|---|---|
| V1 | Can both processes be modified through a legitimate route? | PASS / BLOCKED |
| V2 | Have the exact build/version been identified? | PASS / UNKNOWN |
| V3 | Is there reliable IPC between the processes? | PASS / UNKNOWN |
| V4 | Is there a suitable capture/rendering route? | PASS / UNKNOWN |
| V5 | Is there a sufficient camera for the chosen architecture? | PASS / UNKNOWN |
| V6 | Is there depth/geometry or a justifiable δ route? | PASS / UNKNOWN |
| V7 | Is there sufficient access to the required collision/state? | PASS / UNKNOWN |
| V8 | Do anti-cheat/online/DRM allow the intended scenario? | PASS / BLOCKED |
| V9 | Is the test reproducible offline or in a controlled environment? | PASS / BLOCKED |
| V10 | Is there an oracle that can prove each critical milestone? | PASS / UNKNOWN |

**Rule:** V1 and V8 being BLOCKED ends the project. V2–V7 or V10 being UNKNOWN prevents
irreversible implementation until the uncertainty is reduced or an alternative route is explicitly chosen.

### H0.5 — Evidence Takes Priority Over Assumptions

Every relevant technical claim must carry one of these labels in MODLOG.md:

[VERIFIED]      proven on the target build
[DOCUMENTED]    confirmed by reliable documentation/knowledge
[INFERRED]      technical inference not yet tthisd
[UNKNOWN]       insufficient evidence
[NEGATIVE]      attempt made and proven to fail

A statement such as "ReShade should be able to get depth" is **INFERRED**.
A capture made by ReShade itself showing usable depth is **VERIFIED**.

### H0.6 — Mandatory Version Pinning

Record the following at the start of the project:

GAME_VERSION
GAME_BUILD
ENGINE_VERSION (when known)
MOD_LOADER_VERSION
UNIVERSAL_MODDER_KNOWLEDGE_REVISION (if available)
MINECRAFT_VERSION
MINECRAFT_LOADER_VERSION
OS
GPU
RENDER_API

If a game update changes binaries, loader, renderer, or relevant structures, mark the project
as **version-drift** and repeat the affected Recon. Do not "fix it in the dark" by copying old offsets.

### H0.7 — Circuit Breaker

After approximately **3 equivalent attempts without new evidence**, stop.

Do not make a fourth attempt by randomly changing the DLL, offset, hook, or timing. Instead:

1. record the failure;
2. classify the hypothesis;
3. return to the gate that should have proven this hypothesis;
4. consult Universal Modder again;
5. choose an alternative route or declare BLOCKED.

**Repeated failure without new information is not progress.**

### H0.8 — Scope Frozen by Milestone

Do not try to build the complete passthrough all at once.

The minimum order is:

PROCESSES
   ↓
LOADER / HOOK
   ↓
IPC ROUND-TRIP
   ↓
FRAME / STATE
   ↓
CAMERA
   ↓
DEPTH / GEOMETRY
   ↓
COLLISION
   ↓
INTERACTION
   ↓
PERFORMANCE
   ↓
LIFECYCLE / RECOVERY

Each milestone needs its own oracle. A later milestone must not mask the failure of an earlier one.

### H0.9 — Safety and Legitimacy

This playbook does not authorize bypassing anti-cheat, DRM, integrity protection, authentication,
hardware IDs, protected servers, or equivalent mechanisms.

If the only way to proceed is to circumvent a protection, classify it as **BLOCKED** and stop.

For online games, limit work to explicitly controlled and permitted environments, such as
single-player or a private server when modification is supported.

### H0.10 — Reversibility

Every temporary installation must have:

- a backup;
- an exact list of changed files;
- a restoration method;
- a cleanup process at shutdown;
- a watchdog that does not depend on frame heartbeats to determine whether a process has died.

If the agent cannot explain how to return to the original state, the step is not ready for
automated execution.

### H0.11 — Definition of "Working"

"Opened" does not mean "worked."

A passthrough can only be called **VERIFIED** when the corresponding milestone has been demonstrated by
an oracle. For the complete integration, this normally includes:

- both processes start;
- IPC works in both directions;
- camera/pose are consistent;
- GUEST content appears in the chosen composition;
- depth/occlusion or the δ route works;
- collision/interaction crosses the boundary between worlds;
- essential states survive load/respawn/restart as defined by the scope;
- shutdown does not leave broken hooks/proxies;
- the minimum performance defined in the plan is achieved.

---

---
## Index

**Phase 0 — Reconnaissance (required, before any code)** → [§0](#phase-0--recon-required)
**Phase 1 — Architecture (decide, don't code)** → [§1](#phase-1--architecture)

**Phase 2 — The adapter (the real work, per engine)** → [§2](#phase-2--layer-5-o-adaptador)
- [2.1 Unity Mono](#21-unity-mono--bepinex-5--melonloader)
- [2.2 Unity IL2CPP](#22-unity-il2cpp--bepinex-6)
- [2.3 Skyrim / Bethesda](#23-skyrim--bethesda--skse)
- [2.4 Unreal](#24-unreal--ue4ss--ce4ss)
- [2.5 Capcom RE Engine](#25-capcom-re-engine--reframework)
- [2.6 FromSoftware](#26-fromsoftware--modengine2--me3)
- [2.7 RAGE / GTA / RDR](#27-rage--gta--rdr--scripthookv)
- [2.8 Source 1 / Source 2](#28-source-1--source-2)
- [2.9 .NET / XNA](#29-net--xna--tmodloader--smapi)
- [2.10 Godot](#210-godot)
- [2.11 Native without a loader](#211-nativo-sem-loader--o-caminho-difícil)
- [2.12 Another JVM game](#212-outro-game-jvm)
- [2.13 HTML5 / Electron / NW.js / LÖVE](#213-html5--electron--nwjs--löve)
- [2.14 Data-only engines (answer NO)](#214-engines-de-dados--responda-not)

**Phases 3–8 — Reusable layers (copy, don't rewrite)** → [§3](#phase-3--layer-1--core)
**Phase 9 — Build order (12 steps with gates)** → [§9](#phase-9--ordem-de-construção)
**Phase 10 — Oracles** → [§10](#phase-10--oracles-required-por-passo)
**Consolidated pitfalls** → [§11](#11-armadilhas-consolidadas)
**Per-game checklist** → [§12](#12-checklist-por-game-fillable)
**Appendices** → [§13](#appendix-a--reference-numbers) · [§14](#appendix-b--the-three-ways-to-provide-depth) · [§16](#appendix-d--references)

**Legend:** ✅ verified in decompiled code or a binary · 📚 general engineering knowledge ·
⚠️ documented pitfall or negative result

---

## Quick Reference Map

Three implementations that **exist and work** — their numbers and pitfalls are the foundation for this:

| | Host | Transport | Render | Author |
|---|---|---|---|---|
| **MinecraftInsideULTRAKILL 0.2.0** | ULTRAKILL (Unity Mono, BepInEx+Harmony) | WebSocket JSON + shm 284 MB | **β** depth-mesh | chavi |
| **Minecraft inside GTA V** | GTA V (RAGE, ScriptHookV + ReShade) | WebSocket JSON + shm | **γ** shader | rehan |
| **SkyCraft 0.1.0** | Skyrim SE (native SKSE, DX11, Havok) | **shm 200 MB binary, no JSON** | **δ** guest draws | chasmlol |
| **Portalcraft 0.1.0** | Portal 2 (Source 1, D3D9, **32-bit**) | shm + **camera prediction** | **γ** shader | chasmlol |
| **Terraria (FalArsenal)** | Terraria (tModLoader) | — | data mod | rehan |

⚠️ **This playbook covers one pattern only: two live processes, mods in each, IPC between them.**
The three reference implementations (above) do exactly that and nothing more.

If the idea is "bring **one thing** from X to Y" — a weapon, an enemy, a block — that's a **content
port**: a regular mod in game Y, with no IPC and no two processes. Save yourself time and skip
this document.

Passthrough is for when **the physical interaction between the two is the product** — the car hitting
the wall you built, the enemy from Y shooting Steve while he's on the ground in X.

**If the `mashup-mods` plugin offers a lighter pattern that solves the idea, it's the right choice, and this
playbook doesn't apply.** Tell the user that and stop.

---

# PHASE 0 — RECONNAISSANCE (required)

> **Do not write a single line of code before finishing this phase.** The three implementations that
> worked spent most of their effort here. One of them (Wukong) spent **2 hours on reconnaissance**
> to discover that the entire plan was infeasible — and that was the project's cheapest outcome.

## 0.1 Check the knowledge base first (required when available; 30 s, saves days)

If you have `universal-modder` available:

```bash
um kb search "<game>"          # has this game been done before?
um kb search "<engine>"        # has this engine been solved before?
```

Without `um`, read the index:
`https://github.com/rehan-remade/universal-modder/blob/main/knowledge/INDEX.md`

**If a note already exists:** start with its exact versions, route, and pitfalls. Don't repeat another
agent's dead end.

## 0.2 Identify the engine, loader, and anti-cheat

```bash
um scan "<game>"      # engine+version, managed or native, anti-cheat, saves, loaders, route
```

If you don't have `um`, identify it manually using the table in [§2](#phase-2--layer-5-o-adaptador):

| What you see in the install directory | Engine |
|---|---|
| `UnityPlayer.dll` + `<Game>_Data/` + `Managed/Assembly-CSharp.dll` | Unity **Mono** → §2.1 |
| `UnityPlayer.dll` + `GameAssembly.dll` + `il2cpp_data/Metadata/global-metadata.dat` | Unity **IL2CPP** → §2.2 |
| `Data/*.esm` + `SkyrimSE.exe` / `Fallout4.exe` + `Data/SKSE/` | Creation Engine → §2.3 |
| `Binaries/Win64/<Proj>-Win64-Shipping.exe` + `Content/Paks/` | Unreal → §2.4 |
| `dinput8.dll` + `reframework/` already present | RE Engine → §2.5 |
| `GameAssembly.dll` + `me3.exe` / ModEngine2 | FromSoftware → §2.6 |
| `GTA5.exe` + ScriptHookV | RAGE → §2.7 |
| `gameinfo.txt` + `*_dir.vpk` / `engine2.dll` | Source → §2.8 |
| `<Game>.exe` + `<Game>.dll` in `data_*` or `Managed` (not Unity) | .NET/XNA → §2.9 |
| `.pck` next to the exe or appended to it | Godot → §2.10 |
| Nothing recognizable; C/C++ with `bin/engine.dll` or a custom exe | **native** → §2.11 |
| Main `.jar`, no C++ | JVM → §2.12 |
| `resources/app.asar`, `package.nw` | Electron/NW.js → §2.13 |
| Plain-text data, no real-time 3D rendering | **data-only engine** → §2.14 |

## 0.3 Check the current community setup (5 min)

Versions change. **Don't install from memory.**

- The game's modding wiki, Nexus/Thunderstore/mod.io/Workshop, GitHub
- Which loader the community uses **for this version** (tModLoader, SMAPI, BepInEx, UE4SS,
  REFramework, SKSE, Fabric, ModEngine2/me3)
- If a loader with a hook API already exists → **use it**, don't build one from scratch

## 0.4 ⚠️ SECURITY GATE — pass this before continuing

> **This gate can end the project. Do not bypass it.**

| Situation | Verdict |
|---|---|
| The game has an **online/competitive** component | Single-player only, or a server run by the user |
| **Kernel- or user-mode anti-cheat** (EAC, BattlEye, Vanguard, EA Javelin, Ricochet, ACE, nProtect, XIGNCODE, mhyprot) | ⛔ **STOP.** Code injection is a hard red line |
| The only available route involves **disabling/bypassing** anti-cheat or DRM | ⛔ **STOP.** Do not bypass it |
| Requires **hardware ID spoofing** or tampering with **Denuvo/Steam stub** | ⛔ **STOP** |
| Denuvo is present but **doesn't block** injection | ✅ You can proceed offline (this was the case with Wukong) |
| Offline with anti-cheat, but an **official offline option** exists without it | ✅ Use **only** the official option |

Offline/multiplayer with anti-cheat is forbidden by policy, not because it's difficult. If the user asks
for this, refuse and offer single-player or an official modding surface.

⚠️ **Tools running alongside a protected game can trigger the anti-cheat even if you don't touch the game.** Close
protected games before RE sessions.

## 0.5 The four questions (A1–A4) — answer with evidence

These determine **whether** and **how** it's possible. For each one: what you found, where, and how you confirmed it.

| # | Question | How to answer |
|---|---|---|
| **A1** | Can I read the final frame's **depth**? | Use RenderDoc on a frame. Find the depth attachment. If there's only **one** color attachment and no depth → occlusion compositing is out of reach on this route |
| **A2** | Can I **write the camera pose**? | Find where the view-projection is constructed. In an engine with a native camera → it's the camera object. Unity → the `Transform` of the `Camera`. Skyrim → `NiCamera` |
| **A3** | Can I read the **collision geometry**? | ⚠️ This is the **collision resource, not the render resource.** Unity: `MeshCollider.sharedMesh`. Skyrim: `hkpShape` (Havok). Unreal: `UBodySetup`/`Chaos`. Source: the traces |
| **A4** | Can I **push damage/state**? | A damage method that accepts an override. Find it by looking for what already changes the player's health |

Record this in `MODLOG.md`. **Do not skip A1** — it's the gate for §0.6.
## 0.6 DEPTH GATE — test before planning

> This is the cheapest gate and the one that kills the most projects. Do it **before** writing any composition
> code.

**If your choice is γ (shader):** you **depend** on host depth.

1. Install ReShade with **add-on support**
2. Put `DisplayDepth.fx` + `ReShade.fxh` in the shaders folder
3. `DisplayDepth.fx` with `iUIPresentType == 2` deliberately draws **normals on the left half and
   linearized depth on the right**
4. Check **`Copy depth buffer before clear operations`** AND **`Copy depth buffer during frame to
   prevent artifacts`**, then restart
5. ⚠️ **Capture using ReShade's own screenshot, NOT a window capture** (see pitfall 23.1)
6. Measure how many distinct colors each half contains

**Interpretation:**

```
left half (normals)   distinct colors = 1, everything (127,127,255)   → NOTHING was drawn
right half (depth)    distinct colors = 1, everything (255,255,255)   → depth = 1.0 = EMPTY
```

⚠️ **Measured result in Black Myth: Wukong (UE5, D3D12):** ReShade 6.8 hooks perfectly,
`D3D12CreateDevice` and `CreateSwapChain` are redirected, swapchain `DXGI_FORMAT_R10G10B10A2_UNORM`,
4 buffers — and even so, depth comes back **completely flat**. Reproduced byte-identically
after a full restart, with both options enabled. `ADDON_ADJUST_DEPTH` was checked and ruled out.

| Result | Meaning | What to do |
|---|---|---|
| **Gradient with geometry** | γ is viable | proceed with γ |
| **Flat (everything 1.0)** | γ is **not viable** on this route | go to **δ** (§2 of appendix B) — or find the depth buffer using a custom add-on (net-new, no prior art) |

⚠️ If your engine is UE5/D3D12 and you need γ: **consider δ as the primary plan**, not plan B.

### 0.6b ⚠️ Proxy DLL — WHERE to put it (three documented failures)

This step is error-prone enough, and in enough different ways, to warrant a dedicated section.

A proxy `d3d9.dll`/`dxgi.dll`/`version.dll` **in the wrong place simply never loads** — and the
symptom is "my DLL does nothing, no error." ⚠️ Three cases have already been documented, **all different**:

| Game | Where the proxy **doesn't** work | Where it works | Why |
|---|---|---|---|
| **GTA V** | `dxgi.dll` in the game folder | `ReShade64.asi`, via **ASI loader** | the game loads the **system** `dxgi.dll` before a proxy in its folder |
| **Portal 2** | `d3d9.dll` next to `portal2.exe` | **`bin\d3d9.dll`** | the game loads `shaderapidx9.dll` with an **altered search path** |
| **BepInEx** (Unity) | — | `winhttp.dll` (Doorstop) or `version.dll` (MelonLoader) | the loader chooses |

⚠️ **Procedure before assuming it works:**

1. `dumpbin /imports <game>.exe` (or PE-bear) — see **from which folder and under what name** the game loads
   the module you want to hijack
2. Put the proxy **in the subfolder the game uses**, not the root, if the game loads from a subfolder
   (`bin`, `game/bin`, `<Mod>/`)
3. **Verify that it loaded** — an `OutputDebugString`/`Log` in `DllMain` or a registry key/
   marker file. Without this proof, "did nothing" is indistinguishable from "didn't load"
4. If the native proxy doesn't work, use **Ultimate ASI Loader** (ThirteenAG) — it's a
   ready-made proxy and loads `*.asi`

⚠️ **Never trust "works on my PC."** DLL search paths are influenced by `PATH`, secure
boot, and eKnownDLLs. Test on a clean install.

## 0.7 Set up the lab

```bash
um backup create "<pasta de saves>" --name <game>-saves    #paths come from um scan
```

- **Separate Minecraft profile, mandatory.** The mod overwrites options (clouds, bob, FOV
  effect, fps cap) and creates/opens a world. Give it its own `gameDir`. Otherwise, you'll destroy the user's
  worlds.
- **Windowed at a known client size** (registry/ini) for stable screenshots and coordinates.
- **Decompiled code and extracted assets outside the repo** (`~/<game>-decomp`), with `.gitignore` for
  derived files. Never commit game files.
- **Record the restore path** in `MODLOG.md`.

## 0.8 GATE 0 — proceed only when all of these are filled in

- [ ] KB searched
- [ ] Engine + loader + version identified
- [ ] **Security gate passed** (§0.4)
- [ ] A1 answered **with evidence** (capture or dump)
- [ ] A2, A3, A4 answered
- [ ] **Depth gate completed** (§0.6) — or justified as "not applicable, chose δ"
- [ ] Backup made, lab ready
- [ ] `MODLOG.md` created with paths, versions, decisions

**Output: the RECON REPORT** ([template in §0.9](#09--template--recon-report))

## 0.9 TEMPLATE — Recon Report

```markdown
# Recon: <Game> ↔ Minecraft

## Fixado
- Game: <nome> <version/build>, Steam appid <id>
- Engine: <engine> <version>
- Loader disponível: <which> <version>  /  unavailable
- Anti-cheat: <which> — test mode: <how>
- Saves: <pasta>
- Log do host: <caminho>
- Log do Minecraft: <gameDir>/logs/latest.log

## A1 — Profundidade
<yes/no + evidência: capture, recurso, formato, quem tem o attachment final>
## A2 — Pose de câmera
<where é montada; objeto/método; posso escrever? how?>
## A3 — Geometria de colisão
<recurso; formato; how extraio?>
## A4 — Damage/estado
<método; assinatura; posso interceptar e cancel?>

## Gate de profundidade (se γ)
<resultado medido: cores distintas por metade, conclusão>

## Anti-cheat / legal
<veredicto; rota de lançamento offline>

## Decisão preliminar de rota
<α/β/γ/δ + eixo de câmera, com uma frase de justificativa>
```

---


### 0.7 Mandatory feasibility report

Before entering Phase 1, add a block to MODLOG.md with:

STATUS: 🟢 VERIFIED | 🟡 PARTIAL | 🟠 PROTOTYPE | 🔴 BLOCKED
HOST:
GUEST:
GAME_VERSION:
GAME_BUILD:
ENGINE:
LOADER:
RENDER_API:
ANTI_CHEAT:
IPC:
A1_FRAME_DEPTH:
A2_CAMERA_WRITE:
A3_COLLISION_GEOMETRY:
A4_DAMAGE_STATE:
RENDER_ROUTE:
CAMERA_AXIS:
V1..V10:
UNIVERSAL_MODDER_SOURCES:
OPEN_HYPOTHESES:
KNOWN_NEGATIVES:
NEXT_PROOF:

**Don't jump straight to "probable architecture."** The architecture must be justified by the
results above.

---
# PHASE 1 — ARCHITECTURE

> Decide here. Do not start coding Layer 5 before completing Phase 1.

## 1.1 Axis 1 — Who owns the camera?

| | Description | Cost | Who does it |
|---|---|---|---|
| **A · Minecraft controls** | The player is in Minecraft; the host camera is remotely controlled | requires A2 (writing the pose) | ULTRAKILL, GTA |
| **B · Host controls** | The player is in the host; Minecraft is a showcase/server | **zero** write hooks | SkyCraft |
| **C · Symmetric** | both respond to the keyboard, switching focus | two input consumers; fragile | rare |

⚠️ **Choose B if A2 came back "difficult."** SkyCraft proves that B delivers a complete product
(Skyrim with Minecraft physics, inventory, and viewmodels), and costs less because you don't need
to write to anything in the host — only read.

## 1.2 Axis 2 — Who does the final rendering?

**This is the most important question.** Four answers:

| | Description | Does it need host depth? | Who does it |
|---|---|---|---|
| **α · Overlay** | the frame becomes a texture on a quad | **no** | first milestone, never ships |
| **β · Depth-mesh** | the frame becomes a **real 3D mesh**; the host's Z-test resolves occlusion | **yes** | ULTRAKILL |
| **γ · Shader** | an injected shader in the pipeline tests guest depth against host depth | **yes** | GTA |
| **δ · Guest renders** | the host passes **geometry** to the guest, which renders **inside** the host | **no** | SkyCraft |

### Automatic engine-based routing

| Engine | A1 easy? | **Recommended mode** | Why |
|---|---|---|---|
| Unity Mono | yes (`MeshCollider`) | **β** | full geometry API, `Camera.onPreCull` |
| Unity IL2CPP | yes | **β** | same, via Il2CppInterop |
| Skyrim / Creation | **no** (D3D11) | **δ** | this is what SkyCraft proved |
| Unreal | **risky** (see Wukong) | **δ** if γ fails; **β** via `SceneViewExtension` | UE5+D3D12 has documented flat depth |
| RE Engine | probe first | **β/δ** | REFramework provides Lua with managed types |
| FromSoftware | D3D12 | **δ** | me3 provides DLL hooks; flat depth likely |
| RAGE / GTA | yes (reversed-Z) | **γ** | has precedent, works |
| Source 1/2 | trace-based | **β/δ** | trace-based collision, no mesh |
| .NET / XNA | varies | **β** if D3D9/DX11 is exposed | |
| Godot | yes | **β** | |
| Native with no loader | almost never | **B + δ** or **α** | see §2.11 |
| JVM | N/A | **α or β** | JVM hooks are easy |

⚠️ **Cost-saving rule:** if A1 came back "no," only β and γ are ruled out. **δ doesn't need depth** —
only the matrices, to place geometry in the right location. So δ is the plan, not plan B.
## 1.3 Axis 3 — Transport

| | When |
|---|---|
| **JSON over WebSocket** | discrete state. Always, for state |
| **binary shm** | pixels, geometry, anything large |
| **LAN (e4mc / integrated)** | when the guest is multiplayer (SkyCraft: up to 100) |

⚠️ **Choose JSON+shm (modes A/β/γ) unless the payload is large.** It’s easier to debug and
covers 99% of cases. Only switch to binary shm with named rings if the bandwidth budget is exceeded
(~100 KB/frame of geometry is the point where JSON dies — that’s what SkyCraft did).

## 1.4 Feasibility and effort table

| Situation | Mode | Realistic effort |
|---|---|---|
| Unity Mono/IL2CPP with BepInEx | β | 4–8 weeks |
| **Any Unity + ReShade, no loader** | γ | ~2 weeks |
| Skyrim AE + SKSE (δ) | δ/B | ~1–2 weeks |
| GTA V Legacy + ScriptHookV | γ | ~2 weeks |
| Unreal with UE4SS / C# loader | β or δ | 6–10 weeks |
| RE Engine / FromSoft | δ | 4–8 weeks |
| Source 1/2 | β/δ | 2–4 weeks |
| Godot | β | 2–3 weeks |
| Native without a loader | B + α/δ | 1 week for B; A is unrealistic |
| Other JVM | α/β | ~1 week |

⚠️ **“Week” = part-time, for someone who has modded before.** If this is your first project, double it.

## 1.5 GATE 1 — proceed only with the PLAN written down

- [ ] Camera axis chosen, with justification
- [ ] Mode α/β/γ/δ chosen, with justification (incl. the depth gate result if γ)
- [ ] Transport chosen
- [ ] Game engine: mods? name, path, config
- [ ] What you’re going to do **first** (the vertical slice)
- [ ] Which orACLE will prove step 1
- [ ] `MODLOG.md` with the approach and the reason

**Output: the PLAN** ([template in §1.6](#16--template--plano))

## 1.6 TEMPLATE — Plan

```markdown
# Plan: <Game> ↔ Minecraft

## Architecture
- Camera: <A|B|C> — <reason>
- Render: <α|β|γ|δ> — <reason>
- Transport: <JSON+shm | binary shm | shm+LAN>
- Scale: UNITS_PER_BLOCK = <?>  (measured: host height / Steve’s height)
- Angles: <derived conversion table>

## Layer 5 — the adapter
- A1: <how>
- A2: <how>
- A3: <how>
- A4: <how>

## Vertical slice (first)
<the smallest object that proves the entire path>

## Oracles per step
| Step | Oracle | Where the result appears |

## Out of scope (v1)
<explicit list — prevents scope creep>
```

---

# PHASE 2 — LAYER 5: THE ADAPTER

> **This is the real work.** Layers 1–4 are copied (Phases 3–8). Only this is game-specific.

## 2.0 The contract — write it BEFORE implementing

```csharp
public interface IHostAdapter {
    // A1 — pixel capture
    bool TryCaptureFrame(out HostFrame f);          // color + depth
    // A2 — camera write
    void WriteCamera(in HostPose pose);             // pos, rot, fov, near, far
    // A3 — geometry read
    void PublishGeometry();                         // sends tris/trism/trisdel
    // A4 — push state
    void OnMinecraftDamage(double amount, Vector3 from, bool ranged);
    void OnMinecraftSwing(Vector3 eye, Vector3 dir);
    void OnMinecraftPlace(Vector3 at, Vector3 normal);
    // lifecycle
    void Start();  void Stop();                     // Stop() undoes EVERYTHING
}
```

It serves as both a **checklist** and **documentation** of what you’ve done. Fill in the 4 slots above with
concrete answers before coding.

⚠️ `Stop()` needs to undo every hook. A plugin that leaves a patch installed corrupts the user’s save on
the next launch.

---

## 2.1 Unity Mono — BepInEx 5 / MelonLoader

**The most direct route. It’s the entire ULTRAKILL setup.**

### Install the loader
```
1. Extract BepInEx 5 into the game folder. It adds winhttp.dll (Doorstop proxy)
   and doorstop_config.ini.
2. Run the game ONCE to generate BepInEx/config.
3. Plugins are .NET assemblies in BepInEx/plugins/.
4. Log: BepInEx/LogOutput.log  (enable the console in BepInEx.cfg)
```

### Plugin skeleton
```csharp
using BepInEx;
using HarmonyLib;
using UnityEngine;

namespace MinecraftPassthrough;

[BepInPlugin("voce.seu.mod", "Minecraft Passthrough", "0.1.0")]
public class Plugin : BaseUnityPlugin {
    public static Plugin Instance; public static ManualLogSource Log;

    private class Runner : MonoBehaviour {          // ⚠️ don't use Update on Plugin
        public Plugin p;
        void Update()  => p.Tick();
        void OnGUI()   => p.Gui();
        void OnApplicationQuit() { p.overlay.Disable(); p.link.Stop(); }
    }
    Runner runner;

    void Awake() {
        Instance = this; Log = Logger;
        new Harmony("voce.seu.mod").PatchAll();      // ⚠️ after binding config
        Application.runInBackground = true;          // ⚠️ required
        link = new Link(Port);
        var go = new GameObject("PassthroughRunner") { hideFlags = (HideFlags)61 };
        UnityEngine.Object.DontDestroyOnLoad(go);
        runner = go.AddComponent<Runner>(); runner.p = this;
        Camera.onPreCull += OnPreCull;               // ⚠️ camera hook
        Log.LogInfo("loaded. F10 toggles; play Steve in the Minecraft window.");
    }
}
```

⚠️ **Three rules that can cost you hours if you discover them later:**
1. **Don’t use `Load()`/`Unload()`** from BasePlugin — put everything in `Awake()`, with a nthisd
   `MonoBehaviour` for `Update`/`OnGUI`, `HideFlags = 61` + `DontDestroyOnLoad`.
2. `Application.runInBackground = true` — otherwise the game pauses when focus shifts to Minecraft.
3. `Camera.onPreCull` **plus** a prefix on a host camera system (§2.1.6).

### A2 — camera

```csharp
// 1. the global hook: runs before ALL rendering
Camera.onPreCull += cam => {
    if (cam == MonoSingleton<CameraController>.Instance.cam) ApplyCamera(cam);
};

void ApplyCamera(Camera cam) {
    cam.transform.SetPositionAndRotation(composite.CamPos, composite.CamRot);
    cam.fieldOfView = composite.Fov;
}

// 2. ⚠️ and AFTER any system that moves the camera
[HarmonyPatch(typeof(PortalManagerV2), "LateUpdate")]     // target game name
static class PortalCameraPatch {
    static void Prefix() => Instance.ApplyCamera(CameraController.Instance.cam);
}
```

⚠️ **General rule: apply the pose as late as possible in the host pipeline.** `onPreCull` in
Unity, `Present` in DX11, the last pass before the swapchain. Anything earlier, and some host system
will override you (portals, cutscenes, shake).

### A3 — geometry (colliders only, NEVER renderers)

```csharp
static readonly Collider[] found = new Collider[8192];
const int MASK = /* LayerMaskDefaults.Get((LMD)1) | 1 | 0x40000 */;

int n = Physics.OverlapSphereNonAlloc(steve, Radius * Map.S, found, MASK,
                                       QueryTriggerInteraction.Ignore);

// whitelist
static bool Sendable(Collider c) =>
    c is MeshCollider || c is BoxCollider || c is SphereCollider || c is CapsuleCollider;

// by type:
//   MeshCollider  → mesh.vertices + mesh.triangles (36 B/tri), cached per mesh
//   BoxCollider   → 12 tri
//   Sphere/Capsule→ 112 tri
// ⚠️ mesh.isReadable == false → UNREADABLE piece: mark "hard", send fallback mesh
```

⚠️ **Do not send `mesh.colors32`, `normals`, `uv`.** Position only. Color in passthrough comes from the framebuffer.
⚠️ **Exclude the player’s own blocks** from the stream (`c.transform.parent == blockRoot.transform`),
or you get a feedback loop with a 1-frame delay.

### A4 — damage

```csharp
[HarmonyPatch(typeof(NewMovement), "GetHurt")]          // target game name
static class HurtPatch {
    static bool Prefix(int damage, bool instablack, bool explosion) {
        if (!Plugin.Active) return true;
        if (instablack || damage >= 1000) return true;   // ⚠️ never steal a cutscene death
        if (Time.time < Plugin.Instance.graceUntil) return false;
        Plugin.Send(new { t = "hurt", d = damage * DamageScale });
        return false;                                    // cancel the native handler
    }
}
```

### Hide the host character

```csharp
// ⚠️ use forceRenderingOff, don't rename or destroy
foreach (var r in v1.GetComponentsInChildren<Renderer>(true)) r.forceRenderingOff = true;
// and a duplicate on the mirror layer so mirrors show Steve
```

⚠️ **Don’t use `DontDestroyOnLoad` without reapplying on scene load.** Hook
`SceneManager.sceneLoaded` to reapply state.

### Live inspection
- **UnityExplorer** (BepInEx/MelonLoader plugin): hierarchy, inspector, and in-game C# console. The
  fastest way to find the GameObject and component you need.
- **BepInEx/MelonLoader log** + screenshot.

⚠️ **IL2CPP stripping:** methods the game never called may not exist, and generic instantiations
may be missing.

---

## 2.2 Unity IL2CPP — BepInEx 6

**Requires BepInEx 6 / Il2CppInterop, not 5.**

1. BepInEx 6 generates interop assemblies on first run (slow, one time).
2. Patches use generated `Il2Cpp*` types.
3. Managed `MonoBehaviour`s need `ClassInjector.RegisterTypeInIl2Cpp<T>()`.
4. The API is the same (`MonoBehaviour`, `Mesh`, `Camera`); only access changes.

**Reading:**
```bash
# Cpp2IL or Il2CppDumper with GameAssembly.dll + global-metadata.dat
# → type names, methods, fields, addresses, and dummy DLLs for ILSpy
# method bodies are NATIVE: use Ghidra/IDA with the Cpp2IL scripts to name functions
```

⚠️ Loader build groups are tied to the Unity version. Use what the community recommends for the
version of **this game**.

---

## 2.3 Skyrim / Bethesda — SKSE

**The best-documented hook set in existence. SkyCraft is the reference.**

### Install
- Plugins in `Data/SKSE/Plugins/*.dll`; logs in `Documents/My Games/<Game>/SKSE/`
- **CommonLibSSE-NG** (Address Library) provides structs `RE::PlayerCharacter`, `RE::NiCamera`,
  `RE::TESObjectREFR` with correct offsets for the user's build
- Use MO2 for isolation (a profile per experiment), not manual installation
- Fallout 76 is online-only: no client mods

### A2 — camera and player (native script, xbyak)

```cpp
// SkyCraft hooks the character and camera updates
void PlayerUpdateHook::thunk(PlayerCharacter* pc, float delta) {
    // reads Minecraft state from shm, rewrites pc->position in-situ
    SyncSneak(pc, sneaking, delta);
    PerFrame(pc, delta);
}
void PlayerCameraUpdateHook::thunk(PlayerCamera* cam) {
    ApplyMcFov(cam, mcFov);
    EnsureControlsEnabled();      // ⚠️ Skyrim disables input in menus/cutscenes
}
```

⚠️ **Always re-enable input after applying the pose.** Otherwise, the player gets stuck unable to
move during every cutscene.

### A3 — collision via Havok

```cpp
// SIGNATURES: SkyCraft uses exactly these
void Collision::Collect(const RE::hkpShape* shape, const float*, const float[],
                        const float[], Job& job, int);
void Collision::Harvest(int cx, int cy, int cz);
void Collision::WorkerLoop();          // ⚠️ harvesting is expensive: never run it on the main thread
```

⚠️ **Collision in Skyrim is Havok (`hkpShape`), not the render mesh.** If you use the render mesh,
you'll include doors, trigger volumes, and FX.

### Rendering (SkyCraft δ)

- Rendering hooks via the DX11 swapchain's `Present`/`Draw`
- `SetSkyrimShadows(FrameConstants&, const RE::NiPoint3&)`
- `DrawOverlay(IDXGISwapChain*)`, `EnsureTexture(w,h)`, `InitResources(IDXGISwapChain*)`
- Custom shaders: `ShadowVS`, `ShadowPS`, `SkyShadowProbePS`
- `RE::NiCamera` provides the view-projection matrices

### ⚠️ Skyrim-specific bugs

1. **Game updates break plugins.** Pin the game version, or use Address Library.
2. **Save bloat:** removing scripted mods mid-save causes corruption. Test on a disposable save.
3. **Content Club / "Anniversary"** changes the masters. Know your ESM list.

---

## 2.4 Unreal — UE4SS / UE4SS-like

### Install
- **UE4SS**: `dwmapi.dll` (newer builds) or `xinput1_3.dll` proxy + `ue4ss/` folder next to the exe
- What you get: **Lua** mods (`ue4ss/Mods/<Mod>/Scripts/main.lua`), **`RegisterHook`**
  (`"/Script/Engine.PlayerController:ClientRestart"`), `NotifyOnNewObject`, `FindFirstOf`,
  read/write any UProperty, call UFunctions, `ExecuteInGameThread`; **C++** mods; the
  **Live View** (dumper/inspector); SDK generator (Dumper-7 as an alternative)
- ⚠️ **Custom UE games may need a UE4SS config override for the AOBs.**
- Pure IoStore UE5 requires **retoc**, not repak.

### Identify
`um scan` greps the exe for `++UE5+Release-5.x`.

### A1 — the ⚠️ big risk
See [§0.6](#06-gate-de-profundidade--tthis-before-de-planejar). **UE5 + D3D12 has documented flat depth
(Wukong).** Run the test before planning γ.

If γ fails:
- **β** via a `SceneViewExtension` — you control the pass, and can read the depth UE
  renders through a path you control
- **δ** via `USceneComponent`/proxy actors: create native host actors and let UE light them

### A3 — collision
`UBodySetup` / Chaos (`FBodySetup::AggGeom`) provides the collision mesh without touching assets. Alternative:
raycast against `UWorld::LineTraceSingleByChannel` using grid sampling — less precise, but
works without assets.

### A4 — damage
`UGameplayStatics::ApplyDamage` / `AActor::TakeDamage`. `UGameplayStatics::ApplyPointDamage` for
localized damage.

### ⚠️ Bugs
1. **Paks cooked for the wrong UE version crash on mount.** Blueprint mods break with every update.
2. **`.sig` files verify paks.** Community bypasses exist only for specific single-player games —
   **do not** bypass where it is online anti-tamper.
3. **Many online UE games have EAC/BattlEye** → no injection.

---

## 2.5 Capcom RE Engine — REFramework

- `REFramework` (praydog) as `dinput8.dll` + `reframework/` folder
- **Lua** in `reframework/autorun/*.lua` with access to the managed type system:
  `sdk.find_type_definition`, `sdk.hook`, `sdk.call_native_func`
- In-game object explorer, free camera, VR in many titles
- `REFramework-MCP` exposes the game to an agent

⚠️ **Online:** SF6 ranked, MH lobbies. Local cosmetics only. Never use mods that affect online gameplay.

---

## 2.6 FromSoftware — ModEngine2 / me3

- **`me3`** (successor to ModEngine2, archived but widely used) loads mods from a folder and
  **launches the game offline with EAC disabled**. ⚠️ This is the **only** acceptable way. Never online.
- **Code** mods: DLLs loaded by the mod engine, hooks via **MinHook**
- Data/params: **Smithbox** (successor to DSMapStudio); **WitchyBND** for BND/DCX/BDT containers
- Seamless Co-op uses a separate network

⚠️ The viral clips "Minecraft inside Elden Ring" and "creepers in Dark Souls" (September 2026) were
made with Claude/Opus, and **the method has not been published**. The KB has no note. You're ahead.

⚠️ **Elden Ring is D3D12** → read [§0.6](#06-gate-de-profundidade--tthis-before-de-planejar) before
planning γ. Probably δ.

---

## 2.7 RAGE / GTA / RDR

**There is a precedent: GTA V is where passthrough works.** Copy from there.

### Install
- **Story mode only.** BattlEye protects GTA Online; ScriptHookV refuses to run online.
- **ScriptHookV** + **Ultimate ASI Loader** (`dinput8.dll`, `*.asi` plugins)
- **Natives DB** lists callable game functions
- Assets: **OpenIV** (never edit originals), CodeWalker for maps

### ⚠️ Three things only GTA teaches you

1. **ReShade has to load through the ASI loader.** GTA loads the system `dxgi.dll` **before** a
   proxy in its folder, so the `ReShade64.dll` proxy never loads. Install it as `ReShade64.asi`.
2. **Script lifecycle:**
   - the pause menu **stops** ScriptHookV scripts;
   - the idle cinematic camera starts after ~30 s (call `INVALIDATE_IDLE_CAM` every frame);
   - explosion shake is **not** reported by `IS_GAMEPLAY_CAM_SHAKING`, so
     `STOP_GAMEPLAY_CAM_SHAKING` does not cancel it.
3. **Camera timing:** the script reads the camera for the frame **being prepared**, 1 frame ahead of what's
   on screen. The fix is to **reproject to the previous pose** in the compositor. Measure with a golden
   wall scene (Minecraft-only) against the horizon (GTA-only).

### The γ compositor
`compositor.cpp` (ReShade add-on) uploads the Minecraft texture to the GPU and sets uniforms for
`MCPassthrough.fx`. The shader performs: depth test against GTA's reversed-Z, **reprojection** (rotation, then 6-DoF with ray-march in the depth), relighting from GTA's blurred lighting, color grading, haze,
and edge blur.

⚠️ **dev-c.com rejects scripted downloads** without browser headers. The example's `fetch_deps.sh`
sends User-Agent/Accept.

⚠️ **dev-c.com and ReShade are not redistributed** — the example only automates the download.

---

## 2.8 Source 1 / Source 2

### Identify
Source 1: `gameinfo.txt`, `*_dir.vpk`, `bin/engine.dll`. Source 2: `engine2.dll`,
`gameinfo.gi`.

### Routes
- **Source 1:** VScript (Squirrel) in `scripts/vscripts/*.nut`; Garry's Mod Lua;
  SourceMod+Metamod for servers you run; **Source SDK 2013** builds a real mod
- **Source 2:** Workshop Tools (Hammer 2, ModelDoc, Particle Editor); addons in
  `game/<mod>_addons/`; `-tools` opens the editors; **VScript Lua** in Alyx

### A3 — collision
⚠️ **Source does not expose collision meshes cleanly.** Source 2 has **VRF** (ValveResourceFormat),
which **decompiles map collision**. That's exactly what the authors of `um` used to export CS2 map
collision for a movement clone. This is the route for β/δ in Source.

Cheaper alternative: **trace-based**. Raycast from the host to Minecraft (`engine->TraceLine`)
and send them as barriers — that's what GTA did (160 raycasts/frame, 40 blocks).

### ⚠️ VAC
Modified client on a VAC server = ban. Test with `-insecure` on a local server and **never**
inject into CS2/Dota/Deadlock/TF2 in official matchmaking.
### Oraclesosters
Demos (`.dem`) are great: DemoFile.Net and demoparser libraries extract state per tick.

---

⚠️ **Before writing a native hook, test the script console.** Many Source games expose a
console that accepts commands, and this is **orders of magnitude cheaper** than a hook:

- `hurtme N` (requires `sv_cheats 1`) — damage
- `script GetPlayer()...` — `SetVelocity` (flying, slime jumps), `SetHealth` (healing, creative mode)
- `r_drawviewmodel 0/1` — **hide the weapon while you build**
- `prop_dynamic` with `solid 2`, `rendermode 10`, `SetSize` — invisible solid box to represent
  a guest's block
- `logic_auto` with `OnLoadGame -> Kill` — keeps saves clean

⚠️ **This solves A2 and A4 without any hooks** for a large part of the project. And the console is
scriptable, which makes it an oracle too.

⚠️ **Beware of `sv_cheats 1`:** the side effect is that Steam achievements won't unlock during that session. It's
the user's decision.

⚠️ **Entities don't "drop" to the ground after a portal.** See trap 37: recreate, don't teleport.

## 2.9 .NET / XNA — tModLoader / SMAPI

- **Terraria, Stardew Valley, Celthis, FTL** are .NET (XNA/FNA)
- **tModLoader:** free Steam app (id 1281930). Content in C#: `ModItem`, `ModProjectile`,
  `ModNPC`, `ModSystem`, `ModPlayer`, `ModCommand`
- **SMAPI** for Stardew
- ⚠️ **tModLoader refuses to start unless the free app is in the user's Steam library. Add it; don't
  patch the check.**
- Read with `ilspycmd -p -o ~/<game>-decomp <Game>.exe`

### A lucky break: FNA
Terraria runs on **FNA3D** (D3D9/XNA to D3D11 translation). This means:
- The rendering API is **real D3D11** → the β path works
- ⚠️ **FNA3D `ReadBackbuffer` leaks a full-size staging texture per call** — 17 GB in one take.
  **Never read the back buffer from inside the game.** Capture the window from outside (ffmpeg
  `gfxcapture=hwnd=...`).

### A2 / A3
Inspect `Main.player[0]`, `Main.camera`, and the tile API (`Tile`), or
`Collision.CanCollision` / `SolidCollision` for collision. There is no triangle mesh — collision is
a tile grid. ⚠️ **Map to barriers or shapes, not triangles.**

---

## 2.10 Godot

### Identify
`.pck` next to the exe (magic `GDPC`) or appended to the exe itself. `um scan` reads the version in the header.

### Paths
1. **Godot Mod Loader** (`GodotModding/godot-mod-loader`) if the game has one. Mods are zips in `mods/`
   with a manifest; extensions hook methods via `extend`.
2. **PCK overlay:** `ProjectSettings.load_resource_pack("res://mod.pck")`. Requires code execution
   to call.
3. **`override.cfg`** next to the exe:
   ```ini
   autoload/MyMod="*res://mod/my_mod.gd"
   application/run/main_scene="*res://mod/boot.tscn"
   ```
   ⚠️ works **only** if the script is inside a loaded pack.
4. **Rebuild** the recovered project (GDRE Tools) — personal use only, **never** redistribute.

### A2 / A3
- Camera: hook the `Camera3D` node / `_process`
- Depth: `MeshInstance3D` with `depth_draw_mode = DEPTH_DRAW_ALWAYS`, or render to a
  `SubViewport`
- Geometry: `PhysicsServer3D` / `CollisionShape3D`; `MeshDataTool` for meshes
- Input: `_input()`; `DisplayServer.window_set_mode(WINDOW_MODE_WINDOWED)`

### ⚠️ Bugs
1. **Editor and runtime versions must match** (resource format changes from 4.2 → 4.3).
2. **GDScript 2.0 (Godot 4) ≠ 1.0 (Godot 3).** Different languages.

---

## 2.11 Native without a loader — THE HARD WAY

**This is the most important section of this playbook for AAA games without a loader.** Read carefully.

### Reality check

⚠️ **For native games, Mode B (the host controls the camera) is almost always the only realistic option.** Writing
the camera for a custom engine without a loader is the original problem. If the user wants Mode A, say so
before getting started.

### 1. Get code into the process

**Proxy DLL:** place a DLL named after one the game loads from its own directory:
`version.dll`, `dinput8.dll`, `winmm.dll`, `dxgi.dll`, `d3d9.dll`, `xinput1_3.dll`. It forwards the
real exports and runs your code in `DllMain` (**spawn a thread; do almost nothing under the loader lock**).

⚠️ **Check the game's imports first** with `dumpbin /imports` or PE-bear. Placing the wrong proxy
does nothing.

**Ultimate ASI Loader** (ThirteenAG) is a ready-made proxy that loads `*.asi` (renamed DLLs) from the game
folder or from `scripts/`. Many older games already use it.

Proton/Linux: `WINEDLLOVERRIDES="dinput8=n,b" %command%`.

### 2. Find what to hook

**Static:** Ghidra or IDA via MCP (skill `reverse-engineering`). Start with **strings** (UI text,
log mthisges, asset names) and **imports** (D3D, XInput, file APIs). Follow cross-references
to the function that handles what you want to change. **Name functions and structs as you go, and write it up in
`MODLOG.md`.**

> ⚠️ **RE tip that saved SkyCraft:** its release build **preserved RTTI**. Just by extracting
> strings, you can recover the entire architecture — class, function, and field names, as well as
> source-file paths from the PDB. Before opening Ghidra, **try extracting strings** and see what the
> binary gives you for free.

**Dynamic:** Cheat Engine (value scan → "find what writes" → struct → owner), x64dbg, ReClass.NET,
Frida. MCP versions of CE/x64dbg/Frida exist — **bind to 127.0.0.1**, not `0.0.0.0`.

**Signatures, not addresses:** find functions using an AOB/byte-pattern scan at startup, with wildcards for
relocation, so the mod survives updates. **Have a fallback: log clearly when the pattern isn't
found.**

### 3. Hook

MinHook, SafetyHook, or Microsoft Detours. Match the calling convention exactly (x64 has one;
x86 requires `__thiscall`/`__stdcall`).

⚠️ **Mid-function hooks** (SafetyHook `MidHook`) change registers at a single instruction — useful for
capturing state at the exact point.

⚠️ **Keep hooks tiny.** Do the heavy lifting on your own thread or out of process.

### 4. Draw and interact

⚠️ **This is the most common failure point in native games:** the system DLL `dxgi.dll` often takes precedence over a
proxy in the game folder. Before assuming the proxy works, **test it**. If it doesn't load, use the
Ultimate ASI Loader (that's what GTA required).

- **Overlay/UI:** hook `IDXGISwapChain::Present` (D3D11/12) or `vkQueuePresentKHR`, and draw with Dear
  ImGui. Kiero finds these quickly.
- **ReShade addon API** provides `present`, `draw_indexed`, render-target, and **depth events** without
  you having to write the hooks.
- **Inject 3D into the game's own pass:** use the game's view-projection matrices (find them in constant
  buffers with RenderDoc) and its depth buffer. RenderDoc (+ renderdoc-mcp) shows which pass draws
  what.
- **Input:** hook the game's input handling, or raw input / XInput.


1. **ASLR:** compute addresses from the module base at runtime. **Never** hardcode absolute addresses.
2. **Threads:** engines expect calls on the main thread. Queue them and run them from a hooked
   per-frame function.
3. **Crashes:** log to a file and flush. Install an unhandled-exception filter that writes a minidump.
4. **Updates move everything.** Pin the version for mustlopment and **document which build you support**.

---

## 2.12 Another JVM game

**The easiest path of all.** Both ends are JVM.

- Decompile jars with **Vineflower** / CFR / Procyon, or **Recaf** (edits + decompiles)
- Community loaders: Slay the Spire (ModTheSpire + BaseMod, `@SpirePatch`), Starsector (official
  API), Project Zomboid (Lua + Java)
- No loader: **Java agent** (`-javaagent`) with ASM/ByteBuddy, or **Mixin**

### Direct reuse
You can reuse Layers 1–3 almost **literally**: `HostLink` is a `WebSocketServer` (change the
package), `FrameExporter`/`SharedMemory` are FFM (identical), and the dispatcher is the same. **Only the camera and
collision change.**

---

## 2.13 HTML5 / Electron / NW.js / LÖVE

- **Electron:** `resources/app.asar`. Extract with `npx @electron/asar extract app.asar app/`,
  patch the JS, and repack **or** rename it so the `resources/app/` folder is used.
- **NW.js:** `package.nw` or loose files. Devtools via
  `"chromium-args": "--remote-debugging-port=9222"` in `package.json`.
- **Construct / Phaser / PixiJS:** the logic is plain JS.
- **Browser games you control:** the devtools console is your mod loader.
- **LÖVE (Lua):** the game is a zip (`.love` or appended data). Unpack it and read the Lua. For mods without
  repacking: **lovely** (Lua patch injection) + Steamodded (Balatro uses this).
- **Cocos2d-x / Defold / HaxeFlixel:** see `misc-engines.md`.

### A1/A2
⚠️ **These engines don't have a "host depth buffer" in the 3D sense.** An Electron game is a browser.
γ and β are difficult; **δ is natural** (you can inject `<canvas>` and WebGL). Or α (canvas on top).

---
## 2.14 Data Engines — SAY NO

**These are not passthrough candidates.** They have no real-time 3D rendering to composite.

| Engine | games | actual route |
|---|---|---|
| **Paradox** (Clausewitz/Jomini) | EU4, CK3, HOI4, Stellaris, Victoria 3 | **plain-text script** mods |
| **Genie** | Age of Empires II DE | **data** mod (`.dat` via genieutils) |
| **RPG Maker** | MV/MZ/XP/VX | JS plugins / RGSS |
| **Ren'Py** | visual novels | `.rpy` add-ons |
| **GameMaker** | Undertale, Deltarune | UTMT on `data.win` |

**What to tell the user:** "this game has no 3D renderer to composite, so passthrough does not apply.
The alternative is an in-game data mod — see §2.14.1."

### 2.14.1 What to offer instead

An in-game data mod: a `.mod`/`.txt` script in the mods folder, or an edit to a `.dat` file.
GDScript in a pck overlay, RGSS in `Data/Scripts.rxdata`, JS in `js/plugins/`, GML recompiled with UTMT.
Tools: `genieutils-py` (Genie), GDRE Tools (Godot), UTMT (GameMaker), LibLCF (RPG Maker 2000), `unrpyc`/`unrpa` (Ren'Py).

Exception: **GameMaker YYC and any data engine with a built-in 3D runtime** fall under §2.11 — so they become legitimate passthrough candidates.

### 2.15 Lifecycle orchestrator — the launcher pattern

> **Managing what starts and what shuts down is a design decision, and it costs nothing if you
> think about it beforehand.**

The pattern that works:

```
launcher
  1. installs the proxy/loader files (and knows how to remove them)
  2. starts the hidden GUEST, waits for its "status=running" file
  3. starts the HOST through Steam / platform
  4. when the HOST exits: asks the guest to save and shut down, and REMOVES the proxy files
  5. watchdog: if either side dies, shuts down the other
```

Why this matters:

- **The proxy is temporary.** Removed at the end of the session; it never stays in the user's install. Good for
  security ([§0.4](#04--gate-de-segurança--passe-isto-before-de-continuar)) **and** good for coexistence
  with other mods.
- **The guest runs hidden** — `SDL_HideWindow`, show=false, or just the shm with no window. Nobody sees
  two windows.
- ⚠️ **The correct watchdog monitors the process, not the frame heartbeat.** Documented: *"Minecraft
  quit while Portal 2 was minimized — the watchdog used the frame heartbeat."*
  A minimized game does not produce frames. Check that the `.exe` exists.
- ⚠️ **Fixed username.** *"Achievements and stats reset on every launch — the launcher
  chooses a random `PlayerNNN` name, so there is a new UUID every time."*

⚠️ **Side effect you might choose unintentionally:** entering `sv_cheats 1` in a Steam game
**disables Steam achievements for that session**. That is for the user to decide — tell them, don't decide for them.

---


---

# PHASES 3–8 — REUSABLE LAYERS

> **Copy, don't rewrite.** This is a spec extracted from the binaries of three working implementations.
> It is ~85% of the work and is generic to any game.

| Layer | Where | Index |
|---|---|---|
| 3 · Core | `Link`, `Map`, seqlocks | [§3](#phase-3--layer-1--core) |
| 4 · Protocol | `HostLink`, tables | [§4](#phase-4--layer-2--protocolo) |
| 5 · Transport | `FrameExporter`, `SharedMemory`, ABI | [§5](#phase-5--layer-3--transporte-de-pixels) |
| 6 · Window/input | `WindowOverlay`, `InputBridge` | [§6](#phase-6--layer-4--janela-e-input) |
| 7 · Collision | `UkGeometry`, `TriShape`, `Voxelizer` | [§7](#phase-7--colisão) |
| 8 · Combat | `Combat`, `Enemies`, `MobWar` | [§8](#phase-8--combate-bidirecional) |

---

### 2.16 Installer and assets generated on the user's PC

The cleanest way to solve "I can't distribute assets" is **to distribute nothing and generate them in
the user's install**. What Portalcraft does, and the pattern to copy:

**The installer:**
- is a small zip (~0.4 MB): scripts + the mod jar + the add-on + the shader
- downloads **Temurin**, **Minecraft** (Mojang's version JSON, libraries, and assets with `sha1`
  verified), **Fabric** (meta profile), and **ReShade** (`sha256` pinned)
- ⚠️ **never runs the ReShade installer**: extracts the payload from the end of the central directory record
  inside `ReShade_Setup_*.exe`

**The assets are generated on the user's machine, from their **own Minecraft jar**:**
logo (Minecraft font + white concrete + stone + a drawn portal), icon (isometric grass block),
main menu panel, and four loading screens (test-room wall with Minecraft blocks and a blue and an orange portal). System.Drawing.

⚠️ This is better than "converting from the user's install" because it is **deterministic and small** —
it does not depend on a reverse-engineered asset format.

⚠️ **Installer lesson (cost one release bug):** *"Fabric's profile meta provides `sha1` and
`size` for every library **except** `net.fabricmc:fabric-loader`; the installer interpreted 'no hash'
as 'nothing to fetch'."* Fix: **when an entry has no hash, download `<artifact url>.sha1` from the
Maven repository and verify against that.** And ⚠️ **test launching the installed copy, not just installing it.**

# PHASE 3 — LAYER 1: CORE

## 3.1 Who is the server

**Minecraft is the WebSocket server** (in modes A/β/γ). This is not arbitrary: Minecraft already runs an
integrated server and a reliable tick loop, and you don't want a host failure to take down the world.

```java
private HostLink(int port) {
    super(new InetSocketAddress("127.0.0.1", port));
    this.setReuseAddr(true);
    this.setDaemon(true);
}
static void launch() {
    int port = Integer.getInteger("passthrough.port", 25599);
    instance = new HostLink(port);
    instance.start();
    Passthrough.events = mthisge -> instance.broadcast(mthisge);  // 1 writer, N readers
}
```

The host is the client and reconnects forever:

```csharp
private void Run() {
    while (!stopping) {
        var ws = new ClientWebSocket();
        try {
            ws.ConnectAsync(uri, CancellationToken.None).Wait(3000);
            if (ws.State != WebSocketState.Open) { Thread.Sleep(1000); continue; }
            connected = true;
            var rx = Task.Run(() => Receive(ws));
            while (!stopping && ws.State == WebSocketState.Open && !rx.IsCompleted) {
                wake.WaitOne(50);
                while (outbox.TryDequeue(out var msg))
                    ws.SendAsync(Bytes(msg), WebSocketMthisgeType.Text, true, None).Wait();
            }
        } catch (Exception ex) { if (connected) Log(ex); }
        finally { connected = false; ws.Dispose(); }
        Thread.Sleep(1000);
    }
}
```

⚠️ **`Send()` never blocks:**

```csharp
public void Send(string json) {
    // DROPS mthisges when Minecraft is slow or absent.
    if (connected && outbox.Count <= 2000) { outbox.Enqueue(json); wake.Set(); }
}
```

An unlimited backlog causes OOM when the target game is closed. Dropping mthisges is the correct behavior for
state mthisges.

## 3.2 Heartbeats and dead-instance detection

```java
// shm header, 64 bytes
H_MAGIC = 0, H_VERSION = 4, H_SKYRIM_PID = 8, H_MC_PID = 12,
H_SKYRIM_HEARTBEAT = 16, H_MC_HEARTBEAT = 24;   // GetTickCount64 in both

static boolean active() {
    long beat = LONG.getAcquire(shm, 16L);
    return tickCount() - beat < 8000L;              // 8 s
}
LONG.setRelease(shm, 24L, tickCount());           // every frame
```

⚠️ **Detecting an instance change** — the piece nobody has. If the host PID changes, it restarted,
and all state (geometry, atlas, actors) is invalid:

```java
if (SkyLink.skyrimPid() != lastSkyrimPid) {
    generation++;
    WorldExporter.resendEverything();
    Log.info("SkyCraft: Skyrim instance changed (pid {})", SkyLink.skyrimPid());
}
```

Without this, you are left with geometry from the previous level, visibly wrong and with no error mthisge.
## 3.3 Affine coordinate mapping

```csharp
public static class Map {
    public static float S = 2f;              // host units per block
    public static double BaseX = 4096.0;     // level X range
    public static double BaseY = 100.0;      // ground Y
    public static int Version;               // invalidation token

    // X is MIRRORED — hence the -1/S in the matrix.
    public static Vector3 ToUnity(double x, double y, double z)
        => new((float)((BaseX - x) * S), (float)((y - BaseY) * S), (float)(z * S));

    public static Quaternion Rot(double yaw, double pitch)
        => Frame * Quaternion.Euler((float)pitch, (float)yaw, 0f);

    public static void McAffine(double[] P, double[] t) {
        var f = Frame * Quaternion.Inverse(new Quaternion(0,0,1,0));
        P[0] = -f.m00 / S;  P[1] = f.m01 / S;  P[2] = f.m02 / S;
        P[3] = -f.m10 / S;  P[4] = f.m11 / S;  P[5] = f.m12 / S;
        P[6] = -f.m20 / S;  P[7] = f.m21 / S;  P[8] = f.m22 / S;
        t[0] = BaseX; t[1] = BaseY; t[2] = 0.0;
    }
}
```

⚠️ **Measure `UNITS_PER_BLOCK`; don't guess.** The three projects use **2**, **1**, and **70**:

| | mapping | `UNITS_PER_BLOCK` |
|---|---|---|
| ULTRAKILL | V1 (3.5 u) ≈ 1.8 blocks | **2** |
| GTA | 1 meter = 1 block; GTA (x,y,z) → MC (x, z+off, −y) | **1** |
| SkyCraft | 1 block = 70 SKSE units | **70** |

```csharp
float v1Height = nm.GetComponent<CapsuleCollider>() is CapsuleCollider cc
    ? (cc.height * 0.5f - cc.center.y) * nm.transform.lossyScale.y : 1.75f;
float steveScale = (v1Height / hostUnitsPerBlock) * 0.97f;
```

Apply it on **both** sides — a float that exists in only one place will always diverge:

```csharp
Command($"execute as @a run attribute @s minecraft:scale base set {steveScale:F3}");  // MC
mirrorDouble.Scale = steveScale;                                                   // mirrors
```

**BaseX per level** (so two levels don't overlap):

```csharp
uint h = 2166136261u;                          // FNV-1a
foreach (char c in scene) h = (h ^ c) * 16777619u;
BaseX = (h % 2000 + 1) * 4096.0;
BaseY = 100.0 - Feet(Singleton).y / S;
Version++;
```

## 3.4 Cell key hash (identical on both sides)

```csharp
public static long Key(int x, int y, int z)
    => ((long)(x & 0x3FFFFFF) << 38) | ((long)(y & 0xFFF) << 26) | ((long)(z & 0x3FFFFFF);
```
```java
static long key(int x, int y, int z)   // UkGeometry — 22/20/22 bits, 16-block grid
    => ((long)(x & 4194303) << 42) | ((long)(y & 1048575) << 22) | (z & 4194303);
```

⚠️ Diverging here produces nothing but "Steve walks through the floor at certain corners" — no error, no log.

## 3.5 Seqlocks

All three implementations use them. It's the same pattern.

```java
// WRITER
INT.setRelease(shm, seqOff, seq + 1);      // odd = writing
VarHandle.storeStoreFence();
/* ... fields ... */
VarHandle.loadLoadFence();
INT.setRelease(shm, seqOff, seq + 2);      // even = complete

// READER
for (int i = 0; i < 1000; i++) {
    int s1 = INT.getAcquire(shm, seqOff);
    if ((s1 & 1) != 0 || s1 == 0) { Thread.onSpinWait(); if (i > 100) Thread.yield(); continue; }
    leiaOsCampos();
    VarHandle.loadLoadFence();
    if (INT.getAcquire(shm, seqOff) == s1) return campos;
}
return null;
```

SkyCraft uses `1000` retries for `SKY_STATE`, `100` for `WATER_GRID`, and `16` for `ACTOR_TABLE`. The
number is **how long you want to wait** before rendering a frame without that data.

## 3.6 SPSC byte ring (large payload)

```java
long head = LONG.getAcquire(shm, HEAD_OFF);
long tail = LONG.getAcquire(shm, TAIL_OFF);
long pos  = head % RING_BYTES;
long msg  = (8 + payload + 7) & -8L;                    // align to 8
if (pos + msg > RING_BYTES) { escrevaPad(pos); pos = 0; }
if (RING_BYTES - (head - tail) < msg + 8) return false; // full
/* ... type, payload, header, body ... */
LONG.setRelease(shm, HEAD_OFF, head + msg);
```

⚠️ **Real SkyCraft bug:** wraparound uses `head % 67_108_736` but the space check uses
`67_108_864` — a 128-byte discrepancy. Use **the same constant** in both places.

---

# PHASE 4 — LAYER 2: PROTOCOL

## 4.1 Format

JSON with a single `"t"` discriminator. **Every array becomes a flat array of numbers** — never an array of
objects.

```java
// WRONG: 40 bytes of keys per enemy, allocation per enemy
{"t":"enemies","e":[{"id":1,"x":0.5,"y":2.0,"z":-3.0,"w":0.6,"h":1.8}, ...]}
// RIGHT: 6 doubles per enemy, zero allocations
{"t":"enemies","e":[[1,0.5,2.0,-3.0,0.6,1.8], ...]}
```

## 4.2 Dispatcher

```java
public void onMthisge(WebSocket conn, String mthisge) {
    JsonObject m = JsonParser.parseString(mthisge).getAsJsonObject();
    HostState.heard();                                // keeps the link "attached"
    switch (m.get("t").getAsString()) {
        case "cam":     HostState.update(m); break;
        case "tris":    UkGeometry.put(m.get("id").getAsInt(), m.get("d").getAsString(),
                                       doubles(m.getAsJsonArray("m"))); break;
        case "shaped":  WorldBridge.shaped(ints(m.getAsJsonArray("c"))); break;
        case "hurt":    WorldBridge.hurtPlayer(...); break;
        case "gta": case "gtastate": case "gtainfo": case "director":
            relay(conn, mthisge); break;              // fan-out
        default:
            Minecraft mc = Minecraft.getInstance();
            mc.execute(() -> ClientInput.handle(mc, m));   // any new mthisge becomes a callback
    }
}
```

**Three built-in decisions:**
1. ⚠️ **`case "cmd"` has no `break`** in ULTRAKILL — it falls through to `hb`. Harmless bug, shipped. Don't copy it.
2. **`default:` → client input** keeps Layer 4 stable while you invent mthisges.
3. **Every received mthisge calls `heard()`.** Without this, you need a dedicated heartbeat in both
   directions.

## 4.3 Mthisge table

**Create yours on day 1**, even if 80% of it starts out empty.

**Host → Minecraft:**

| `t` | Payload | Handler |
|---|---|---|
| `cam` | full camera pose | `HostState.update` |
| `tris` | `id`, `d` (b64 LE f32), `m[12]` | static geometry |
| `trism` | `b` (b64 LE doubles, stride 13) | **moved** piece |
| `trisdel` | `ids[]` | removal |
| `trisclear` / `holes` | — / `b[]` (6 per hole) | clear / portals |
| `shaped` | `c[]` (4 ints: x,y,z,code) | block grid |
| `unsolid` | `c[]` (3 ints) | clear cells |
| `clear` / `cmd` / `hb` | — / `c` / — | |
| `hold` | `on` bool | freezes Steve |
| `enemies` | `e[][]` (6 doubles) | enemy avatars |
| `hurt` / `heal` | `d`, `pos[3]`, `proj` | damage |
| `blocksync` | `r` radius | re-sync |
| `projhit` | `id`, `pos[3]`, `stick` | projectile |
| `spawnmobs` | `k`,`n`,`rmin`,`rmax`,`arc`,`yaw`,`at[3]` | waves |

**Minecraft → Host:**

| `t` | Payload | Effect |
|---|---|---|
| `steve` | `pos, ground, yaw, hp, max, food, gm, dead, held, gs, vel` | pose + HUD |
| `swing` | `eye[3], look[3], item, creative` | attack / click |
| `uhit` | `id, d, from[3], kind, item, air, sprint, riptide, glide, mob` | melee → damage |
| `proj` | `[id, kind, x, y, z, dmg, mob]` | projectile |
| `explosion` | `pos[3], r, src` | real explosion |
| `skin` | `name, slim, w, h, rgba(b64)` | skin for mirrors |
| `blocks` | `set[3n], clear[3n]` | your builds |
| `died` / `totem` / `blocked` / `inportal` / `pteleport` | | |

**Layer 5** (you invent): `place`, `warp`, `launch`, `key`, `slot`, `scroll`, `hud`, `view`, `look`.

## 4.4 Mode δ: binary with named regions

When the payload is large, JSON won't work. Declare a **region layout**:

```java
public static final int    MAGIC = 1129925459;
public static final String MAPPING_NAME = System.getProperty("skycraft.link", "Local\\SkyCraft_v1");
public static final double UNITS_PER_BLOCK = 70.0;
public static final long OFF_HEADER = 0, OFF_SKY_STATE = 256, OFF_MC_STATE = 512;
public static final long OFF_WATER_GRID = 1024, OFF_OVERLAY_CTL = 768, OFF_OVERLAY_SLOT_HDR = 832;
public static final long OFF_INPUT_RING = 4096, OFF_ACTOR_TABLE = 73728, OFF_EVENT_RING = 94208;
public static final long OFF_WORLD_ENTITIES = 114688, OFF_COLLISION_RING = 131072;
public static final long OFF_OVERLAY_PIXELS = 33685504, OFF_RENDER_RING = 133218304;
public static final long MAPPING_BYTES = 200327168L;
```

**The arithmetic checks out — verify it on paper before coding:**

```
131 072 + 33 554 432           = 33 685 504
33 685 504 + 3 × 33 177 600   = 133 218 304
133 218 304 + 67 108 864       = 200 327 168
33 177 600 = 3840 × 2160 × 4
```

**Debugging advantage:** the offsets have names — `SS_POS_X`, `MS_EYE_HEIGHT_T`, `WE_SEL_MIN`. You
can explain each byte in one sentence. Poorly documented JSON can't.

---

# PHASE 5 — LAYER 3: PIXEL TRANSPORT

## 5.1 Why shm, not JPEG

A 4K RGBA frame is ~33 MB. Base64 over WebSocket at 300 ms latency won't deliver 60 fps.
`copyTextureToBuffer` + `MemorySegment.copy` writes **directly to the mapping**, with no heap array.

If you need compression, use **hardware BCn/ASTC/ETC2**. Don't use JPEG/H.264 in the critical path.
## 5.2 The ABI

```
Name:      Local\MCPassthroughFrame
Size:      298 602 496 bytes (284 MB) = 4096 + 3 slots × 3 planes × 33 177 600
Magic:     1414546253 = 0x5450434D = "MCPT"
```

**Global header:**

| Offset | Type | Value | Meaning |
|---|---|---|---|
| 0 | int32 | `0x5450434D` | magic |
| 4 | int32 | 1 | version |
| 8 | int32 | 4096 | header |
| 12 | int32 | 3 | slots |
| 16 | int64 | 99 532 800 | stride |
| 24 | int32 | 3840 | max width |
| 28 | int32 | 2160 | max height |
| 32 | int64 | — | publish counter |
| 40 | int32 | −1 | published slot |
| 44 | int32 | — | **Minecraft PID** (the host finds the HWND from it) |

**Slot descriptor** — `256 + 128 × slot`:

| Offset | Type | Field |
|---|---|---|
| +0 | int64 | `seq` — **odd = writing, even = valid** |
| +8/+16 | int64 | frame on producer / on host |
| +24/+28 | int32 | `W`, `H` |
| +32/+36 | float32 | near (0.05), far (live) |
| +40 | float32 | **vertical FOV in degrees** |
| +44 | int32 | flags |
| +48/+56/+64 | float64 ×3 | camera position in the MC world |
| +72/+76/+80 | float32 ×3 | yaw, pitch, roll |
| +84 | int32 | first-person? |
| +88/+96 | int64 | capture / publish nanos |
| +104/+112/+120 | float64 ×3 | player **feet** (may be NaN) |

**Payload** — `4096 + 99 532 800 × slot`:

```
+0             color       RGBA8, W*H*4
+W*H*4         depth       float32
+2*W*H*4       overlay     RGBA8   (HUD, chat, hand, UI)
```

**Flags:** bit0 = depth in `[0,1]`; bit1 = reserved (write `2`); bit2 = reversed-Z.

**Why 3 planes:** the producer **clears the color to transparent** after copying, so the HUD/chat/
hand go onto a transparent backdrop—and that's the second plane.

## 5.3 The producer (Java FFM)

```java
private static final Linker LINKER = Linker.nativeLinker();
private static final SymbolLookup K32 = SymbolLookup.libraryLookup("kernel32", Arena.global());
private static final MethodHandle CREATE = LINKER.downcallHandle(
    K32.find("CreateFileMappingW"), FunctionDescriptor.of(ADDRESS, ADDRESS, ADDRESS,
        JAVA_INT, JAVA_INT, JAVA_INT, ADDRESS));
private static final MethodHandle MAP = LINKER.downcallHandle(
    K32.find("MapViewOfFile"), FunctionDescriptor.of(ADDRESS, ADDRESS,
        JAVA_INT, JAVA_INT, JAVA_INT, JAVA_LONG));

static SharedMemory open(String name, long size) {
    var arena = Arena.global();
    byte[] utf16 = (name + "\0").getBytes(StandardCharsets.UTF_16LE);
    var wide = MemorySegment.ofArray(utf16).reinterpret(utf16.length);
    var handle = (MemorySegment) CREATE.invoke(
        MemorySegment.ofAddress(-1L), MemorySegment.NULL, PAGE_READWRITE,
        (int)(size >>> 32), (int)size, wide);
    var view = (MemorySegment) MAP.invoke(handle, FILE_MAP_ALL_ACCESS, 0, 0, size);
    return new SharedMemory(view.reinterpret(size));
}
```

Requires `--enable-native-access=ALL-UNNAMED`.

⚠️ **Only 7 syscalls total** (SkyCraft): `OpenFileMappingW`, `MapViewOfFile`, `GetTickCount64`,
`GetCurrentProcessId`, `QueryPerformanceCounter`, `QueryPerformanceFrequency`, `CreateMutexW`.

⚠️ **Use `nanoTime`, never `GetTickCount`.** `GetTickCount` has ~16 ms steps and causes **judder** in frame
pacing. `nanoTime` == `QPC` on Windows.

## 5.4 The ring (anti-tearing)

```java
static void publish(Capture c) {
    int slot = slotNext; slotNext = (slotNext + 1) % 3;
    MemorySegment m = shm.segment;
    long desc = 256L + 128L * slot;

    long seq = m.get(LONG, desc);
    if ((seq & 1L) != 0L) seq++;        // never overwrites an odd value in flight
    m.set(LONG, desc, seq + 1L);        // 1. ODD = writing
    VarHandle.fullFence();              // 2. release
    long base = 4096L + 99_532_800L * slot;
    int n = c.width * c.height * 4;
    copy(c.color, m, base, n);          // GPU → shm, DIRECT
    copy(c.depth, m, base + n, n);
    copy(c.overlay, m, base + 2 * n, n);
    /* ... descriptor ... */
    VarHandle.fullFence();              // 3. release
    m.set(LONG, desc, seq + 2L);        // 4. EVEN = complete
    m.set(INT, 40L, slot);
    VarHandle.fullFence();
    m.set(LONG, 32L, ++publishCounter);
}
```

```csharp
public bool Next(out Frame f) {
    long counter = *(long*)(b + 32);
    if (counter == lastPublish) return false;
    int slot = *(int*)(b + 40);
    if (slot < 0 || slot >= *(int*)(b + 12)) return false;
    long seq = *(long*)(b + 256 + 128 * slot);
    if ((seq & 1) != 0) return false;
    Thread.MemoryBarrier();                           // acquire
    /* ... parse ... */
    if (f.W <= 0 || f.H <= 0) return false;
    lastPublish = counter;
    return true;
}
public bool StillValid(in Frame f) {
    Thread.MemoryBarrier();
    return *(long*)(b + 256 + 128 * f.Slot) == f.Seq;
}
```

## 5.5 Three required robustness measures

**a) Stuck-slot watchdog** (lost GPU fence = pipeline stalled forever):
```java
private static final long STUCK_NANOS = 1_000_000_000L;
if (!c.busy || now - c.busySince >= STUCK_NANOS) {
    c.generation++; c.busy = true; c.busySince = now;   // generation invalidates delayed callbacks
}
/* and in the callback: */ if (c.generation != generation || !c.busy) return;
```

**b) Size limit:**
```java
if ((long) w * h * 4L > 33_177_600L) { warnOnce(); return; }
```

**c) No CPU fallback.** If `CreateFileMappingW` fails once, export is disabled forever:
```java
} catch (Throwable t) { failed = true; LOG.error("frame export disabled", t); return false; }
```

A synchronous `glReadPixels` drops the framerate to 5 fps. **Better not to integrate anything than to integrate
it slowly.**

## 5.6 Adapting for another producer

**Vulkan:** `vkCmdCopyImageToBuffer` with `VK_IMAGE_ASPECT_DEPTH_BIT` → `R32_SFLOAT`; or
`VK_EXT_copy_memory_indirect` if the image has `READ_ONLY_LATEST_ACCESS`.

**DX12:**
```cpp
D3D12_GPU_COPY_DESC dst = {};
dst.pResource = dstBuffer; dst.Type = D3D12_RESOURCE_TYPE_BUFFER;
dst.SubresourceLayout.Footprint = { DXGI_FORMAT_R32_FLOAT, w, h, 1, w*4 };
copyQueue->CopyTextureRegion(&dst, 0, 0, 0, srcDepth, &srcBox);
```

**OpenGL:** `glReadPixels` from the depth attachment is not portable—**render the depth to an `R32F` color
FBO** in a separate pass.

⚠️ **If you capture color AFTER depth in Minecraft's GL backend, the color comes back as garbage.** The
depth `copyTextureToBuffer` leaves the read buffer set to `GL_NONE` and never restores it. Three-line
fix:

```java
// GlCommandEncoderMixin — only for a source with a depth aspect
@Inject(method = "copyTextureToBuffer(...)",
        at = @At(value = "INVOKE", target = "...GlStateManager;_glFramebufferTexture2D(IIIII)V"))
private void restoreReadBuffer(GpuTexture src, GpuBuffer dst, long off, Runnable cb,
        int mip, int x, int y, int w, int h, CallbackInfo ci) {
    if (src.getFormat().hasDepthAspect()) GlStateManager._glReadBuffer(36064);  // GL_COLOR_ATTACHMENT0
}
```

This was the first passthrough GTA gotcha, and it reappears in any project.
## 5.7 Mode δ — Minecraft draws nothing

Layer 3 in δ transports: **the texture atlas** (once + `REN_ATLAS_REGION` for animated
frames), **32 B/vertex geometry**, and **a UI plane** (the MC HUD needs to appear).

```java
@Mixin(LevelRenderer.class)
class LevelRendererMixin {
    @Inject(method = "render(...)", at = @At("HEAD"), cancellable = true)
    void skipWorld(..., CallbackInfo ci) { if (SkyClient.linked()) ci.cancel(); }
}
```

⚠️ **This answers “what if Minecraft runs at 10 fps?”** In δ, **it doesn’t matter.** Minecraft is an asset
and physics server; the host does the drawing, at 60 fps.

**The texture trick that makes δ work** — steal the atlas from memory:

```java
@Accessor("texturesByName") Map<Identifier, TextureAtlasSprite> skycraft$texturesByName();
@Accessor("originalImage") NativeImage skycraft$originalImage();   // ← PRE-atlas
```

`originalImage` gives you the image **before** it is stitched into the atlas: no need to reparse the PNG, and the animated
frame strip stays intact. Combine blocks+items into one sheet, **edge-replicate** so the sampler doesn’t pull the neighboring
color in the mip chain, and send only UVs:

```java
int pad = Math.max(0, Math.min(imageX - sprite.getX(), imageY - sprite.getY()));
for (int y = -pad; y < h + pad; y++)
    for (int x = -pad; x < w + pad; x++) {
        int argb = image.getPixel(imageX + Math.clamp((long)x, 0, w-1),
                                  imageY + Math.clamp((long)y, 0, h-1));   // edge replicate
        pixels.put((byte)(argb>>16)); (byte)(argb>>8)); (byte)argb; (byte)(argb>>>24);
    }
```

⚠️ **Send the light nibbles (block/sky) per vertex.** Without them, the host shader won’t reproduce MC’s
smooth-lighting term.

⚠️ **Bake MC shading into the vertex** (`unshade(argb, shade)`) and the host shader only needs to
add its own light over an already-shaded base — no Minecraft shader needs to be ported.

**32-byte vertex format:**

```
 0  f32 x,y,z        position
12  f32 u,v          UV in the merged atlas
20  u8  R,G,B,A      color (MC shading already applied)
24  u32 light        block | (sky << 8)
28  u32 flags        bit0 opaque, bit1 translucent, bits4-7 = Direction.ordinal()+1
```

---

## 5.8 Reprojection and camera prediction

> **This is the most conspicuously missing section of the playbook and the biggest whichity jump between “playable” and “smooth.”**
> The GTA passthrough solved the 1-frame lag by *accepting* a frame of delay. Portalcraft
> *predicts* and corrects. Measured: with the user’s settings, **70–80% of presented frames
> were drawn using the previous `OverrideView` camera**.

### 5.8.1 Read the REAL on-screen camera, not the engine camera

⚠️ **The engine hook gives you the camera for the frame *being prepared*. The on-screen camera is different.**

The technique that solves this: **read the view-projection directly from the vertex shader constants.** With the ReShade
add-on, you have `push_constants`; the matrix is in `c8..c11`.

```
view-projection matrix in push constants = the camera the host actually used for this pixel
```

Then: keep a history of the cameras you hooked and match the read matrix to the corresponding entry.
This is **engine-agnostic** — it works with any host that exposes shader constants.

⚠️ If your host doesn’t have ReShade: look for the same matrix in a constant buffer using **RenderDoc**, or
apply the predictor from §5.8.2 even without it (it’s an EMA; it doesn’t need the matrix).

### 5.8.2 The predictor

Don’t compensate for the delay — **predict** and correct:

```
now              = time of the on-screen frame
rendered_for     = time of the camera the guest rendered with
delay            = now - rendered_for                 ← EMA, not the raw value
target_camera    = predicted camera at (rendered_for + delay)
→ tell the guest to render for target_camera
→ in the composite, reproject to the camera that is ON SCREEN
```

⚠️ **The key:** the camera you send to the guest is *predicted*; the camera you
reproject to is *the one on screen*. They are two different cameras. Confusing them is the classic bug.

### 5.8.3 The reprojection algorithm (the expensive detail)

⚠️ **Don’t start the ray march at the pixel “at infinity.”** Where that ray doesn’t hit anything — and the
host’s walls are generally not at the guest’s depth — the search **stops**, and you lose
a wide band of parallax. ⚠️ *“A 16-step march in 1/depth still skipped anything beyond ~6 blocks.”*

**The algorithm that works, in three steps:**

```
1. TILE PASS (coarse, 1/16 resolution)
   For each tile, store the MINIMUM DEPTH of the guest content.
   This limits how close anything along the ray path can be.

2. NEAR-TO-FAR TRAVERSAL (≈1 px per step)
   Given the ray and the tile’s minimum depth, step from near to far in ~1 px increments
   and find the FIRST crossing.
   → starting near, not far, is what ensures the search doesn’t stop in empty space.

3. THICKNESS TEST
   After the crossing, test whether you crossed something “behind an outline” and discard it.
   Without this, reprojection finds edges and turns them into jaggies.
```

⚠️ **Overscan is mandatory with prediction.** Prediction error pushes the picture beyond the
edge. Solution: the guest renders with **`tan(fov/2) × 1.08`** (8% overscan); when sampling,
**color bilinear, depth point** (interpolating depth creates halos).

⚠️ **Player input needs to be visible.** Raycast from the **camera**, not the eye, in
third person — the host camera is offset from the player’s eye.

### 5.8.4 Tests that prove reprojection is correct

Don’t trust your eyes. Measure:

- **Golden pillar**: a guest-only object at a known depth, against a
  host-only background. The analytically projected position and the observed position should match within
  **~1 px**
- **Depth vs native raycast**: the depth you read from the frame **must ewhich** the
  value returned by the host’s own raycast. Measured: `5.5000` vs `5.5000` blocks. Zero difference
- **Strafe and turn**: walk in a zigzag and turn quickly. An outline cut off on one side = §5.8.3
  is broken

# PHASE 6 — LAYER 4: WINDOW AND INPUT

## 6.1 Window overlay (mode A)

```
GWL_STYLE    → WS_POPUP | WS_VISIBLE
GWL_EXSTYLE  → WS_EX_LAYERED | WS_EX_TRANSPARENT | WS_EX_TOPMOST
             | WS_EX_NOACTIVATE | WS_EX_TOOLWINDOW
SetLayeredWindowAttributes(hwnd, 0, 255, LWA_ALPHA)
SetWindowPos(hwnd, HWND_TOPMOST, x, y, w, h, SWP_NOACTIVATE|SWP_FRAMECHANGED|SWP_SHOWWINDOW)
SetForegroundWindow(minecraftHwnd)      ← INPUT GOES TO MINECRAFT
```

## 6.2 Find the windows

The Minecraft PID comes from the shm itself (offset 44):

```csharp
static IntPtr FindWindowOf(int pid, Func<string,string,bool> accept) {
    IntPtr found = IntPtr.Zero;
    EnumWindows((h, _) => {
        GetWindowThreadProcessId(h, out var p);
        if (p != pid || !IsWindowVisible(h)) return true;
        var sb = new StringBuilder(256); GetWindowTextW(h, sb, 256);
        var cb = new StringBuilder(256); GetClassNameW(h, cb, 256);
        if (accept(sb.ToString(), cb.ToString())) { found = h; return false; }
        return true;
    }, IntPtr.Zero);
    return found;
}
var uk = FindWindowOf(Process.GetCurrentProcess().Id, (t,c) => c == "UnityWndClass");
var mc = FindWindowOf(frames.McPid,                   (t,c) => t.StartsWith("Minecraft"));
```

## 6.3 Keep-alive + windowed

```csharp
if (IsIconic(mc)) return;
if ((GetWindowLongPtr(uk, GWL_STYLE) & WS_MINIMIZE) != 0 || !HasFlag(WS_VISIBLE)) ReStyle();
GetClientRect(mc, out var r); ClientToScreen(mc, out var p);
SetWindowPos(uk, HWND_TOPMOST, p.X, p.Y, r.right, r.bottom, 112);
```

⚠️ **Force windowed mode before anything else:**
```csharp
if (Screen.fullScreenMode != FullScreenMode.Windowed) {
    savedMode = Screen.fullScreenMode;
    Screen.fullScreenMode = FullScreenMode.Windowed;
    return false;   // try again on the next tick
}
```
Exclusive fullscreen does not support functional `WS_EX_TRANSPARENT`. This is the #1 cause of “the overlay doesn’t appear.”

## 6.4 Global keys

```csharp
[DllImport("user32.dll")] static extern short GetAsyncKeyState(int vKey);
static bool Tapped(int vk, ref bool wasDown) {
    bool down = (GetAsyncKeyState(vk) & 0x8000) != 0;
    bool edge = down && !wasDown; wasDown = down; return edge;
}
## 6.5 Inject input into the guest (mode B)

When the **host** has focus, inject into **SDL** — where Minecraft actually reads input:

```java
case IN_KEY(1):
    key(handle, code, a != 0); updateModifiers();
    int action = down && !wasDown ? 1 : (down && wasDown ? -1 : 0);
    int keycode = SDLKeyboard.SDL_GetKeyFromScancode(scancode, (short)modifiers, true);
    keyboardHandler.keyPress(handle, action, new KeyEvent(scancode, keycode, modifiers));
case IN_MOUSE_BUTTON(2): mouseHandler.onButton(handle, new MouseButtonInfo(code, modifiers), a != 0 ? 1 : 0);
case IN_SCROLL(3):      mouseHandler.onScroll(handle, 0.0, a / 120.0);   // WHEEL_DELTA
case IN_CURSOR(4):      mouseHandler.onMove(handle, a, b, dx, dy);
case IN_TEXT(5):        if (gui.screen() != null)
                            keyboardHandler.textInput(handle, new String(Character.toChars(a)));
case IN_RELEASE_ALL(6): releaseAll();
case IN_OPEN_MENU(8):   if (gui.screen() == null) { releaseAll(); gui.setScreen(new PauseScreen(true)); }
```

And the mixin that makes **all** MC key polling read from the host:

```java
@Inject(method = "isKeyDown", at = @At("HEAD"), cancellable = true)
static void keyDown(InputConstants self, int key, CallbackInfoReturnable<Boolean> cir) {
    if (SkyClient.tookOver()) cir.setReturnValue(InputBridge.isKeyDown(key));
}
// grabMouse / releaseMouse: canceled while tookOver
```

⚠️ **`RELEASE_ALL` is not optional.** Without it, keys get stuck when the host gains focus. Call it on:
`IN_RELEASE_ALL`, `menuOpen`, `loading`, link drop, and before opening the pause screen.

⚠️ **Fix focus handling:**
```java
@Inject(method = "isFocused",  at = @At("HEAD"), cancellable = true) → linked() when tookOver
@Inject(method = "isIconified", at = @At("HEAD"), cancellable = true) → false while linked
// Screen: isPauseScreen → false while linked
```
If MC thinks it has lost focus, it pauses and you lose the world.

## 6.6 Overlay vs. injection

| | Overlay | Injection |
|---|---|---|
| Who has focus | Minecraft | Host |
| Effort | low (pure Win32) | high (hook into input stack) |
| Risk | style does not apply | stuck keys, accidental pause |
| Used by | ULTRAKILL, GTA | SkyCraft |

An overlay solves 80% of cases.

---

# PHASE 7 — COLLISION

## 7.1 The rule

**Do not replace the collision system. Add a provider alongside the original.**

`VoxelShape` is an abstraction: anything that responds to `collide(Axis, AABB, distance)` can be
added to the list.

```java
// EntityCollideMixin — an EXTRA shape
List<VoxelShape> shapes = original.call(level, source, area);
TriShape tris = UkGeometry.collides(source) ? TriShape.of(area) : null;
if (tris == null) return shapes;
List<VoxelShape> out = new ArrayList<>(shapes.size() + 1);
out.addAll(shapes);
out.add(tris);              // ← appended, not substituted
return out;
```

All native swept movement (substeps, step height, ground) continues unchanged.

## 7.2 Broad phase (two levels)

```
Level 1 — world, 16-block cell
  Long2ObjectOpenHashMap<int[]>   cell → pieces
  > 64 cells per piece  →  "big" list (always tthisd by AABB)
  query > 512 cells   →  "big" only
Level 2 — per piece, within the mesh
  count <= 16  →  linear scan
  otherwise    →  uniform grid, cell = max((hi−lo)/24, 0.001)
  query > 4096 cells  →  linear
```

The bail-outs are **intentional**: conservative and slow in a rare case is better than a giant grid.

## 7.3 Partial shapes (where the actual geometry is not readable)

Per-cell algorithm:
1. `OverlapBox` — no hit → empty
2. **5 downward rays** (center + 4 corners at ±0.45), accepts `Dot(normal, up) > 0.3` → **positive**
   code 1..16 = floor slab of `n/16`
3. **5 upward rays**, accepts `Dot(normal, up) < −0.3` → **negative** −1..−15 = ceiling slab
4. None → 16 (full)

Range: **−115..116**. `+100` = "hard" (unreadable mesh), never yields to triangles.

⚠️ **SkyCraft alternative encoding** (more compact, for when geometry exists but you want cheap block-based
collision for mobs):
```
FILL = bits 0-9 : count of solid voxels
       bit 10    : geometry in the lower half
       bit 11    : geometry in the upper half
       bits 12-14: index of the highest non-empty layer
groundTop(pos) = ((fill >> 12 & 7) + 1) / 8f
```

## 7.4 SAT inside the vanilla engine

**13 axes** — verified in the source:

```
1–3:  box axes              (1,0,0) (0,1,0) (0,0,1)
4:    triangle normal       n = e0 × e1
5–13: each of the 3 edges e: (0,−e.z,e.y) (e.z,0,−e.x) (−e.y,e.x,0)
```

```java
private static boolean axis(double lx,double ly,double lz,int a,
                            double cx,double cy,double cz,double hx,double hy,double hz,
                            double v0x,double v0y,double v0z,
                            double v1x,double v1y,double v1z,
                            double v2x,double v2y,double v2z, double[] lo,double[] hi) {
    double len = Math.sqrt(lx*lx + ly*ly + lz*lz);
    if (len < 1.0E-12) return true;               // degenerate axis: pass
    lx /= len; ly /= len; lz /= len;
    double t0 = lx*v0x+ly*v0y+lz*v0z, t1 = lx*v1x+ly*v1y+lz*v1z, t2 = lx*v2x+ly*v2y+lz*v2z;
    double tmin = Math.min(t0, Math.min(t1, t2));
    double tmax = Math.max(t0, Math.max(t1, t2));
    double r = hx*Math.abs(lx) + hy*Math.abs(ly) + hz*Math.abs(lz);
    double c = lx*cx + ly*cy + lz*cz;
    double from = tmin - r + 1.0E-7 - c;          // EPS shrinks
    double to   = tmax + r + 1.0E-7 - c;
    if (from >= to) return false;                 // separating axis
    double la = a == 0 ? lx : (a == 1 ? ly : lz);
    if (Math.abs(la) < 1.0E-12) return from < 0.0 && 0.0 < to;
    double s0 = from / la, s1 = to / la;
    if (la < 0.0) { double t = s0; s0 = s1; s1 = t; }
    if (s0 > lo[0]) lo[0] = s0;
    if (s1 < hi[0]) hi[0] = s1;
    return lo[0] < hi[0];
}
```

⚠️ **The policy is deliberately tolerant** — that is what makes a ramp feel smooth:

```java
if (d > 0.0) {
    if (s1 <= 0.0 || s0 >= d)  return d;
    if (s0 < 0.0)  return -s0 < 0.02 ? 0.0 : d;   // already inside: only block if grazing
    return Math.min(d, s0);
} else {
    if (s0 >= 0.0 || s1 <= d)  return d;
    if (s1 > 0.0)  return s1 < 0.02 ? 0.0 : d;
    return Math.max(d, s1);
}
```

`PENETRATION = 0.02`, `EPS = 1.0E-7`. Returns **limited distance**, not the actual MTV.

**`getCoords(Axis.Y)` — the detail that gives ramps the correct ground height:** **Sutherland–Hodgman**
clipping of each triangle against the region's 4 side faces, takes the `maxY` of the clipped polygon, then sorts.

```java
if (heights == null) {
    for (int o = 0; o < tris.length; o += 9) {
        double top = clippedTop(tris, o, region);
        if (top >= region.minY && top <= region.maxY) out.add(top);
    }
    out.sort(null); heights = out;
}
```

⚠️ **Push-out (SkyCraft):** Sutherland–Hodgman twice (lower and upper planes), then closest point using
point-in-polygon by signed area, and applies **only the deepest penetration**, once. Without this, when
touching two walls, the player vibrates between them.

⚠️ **`walkable` by normal:** `|ny| >= 0.7`. Floors only start blocking **above the step height**
(`wallFrom = wasOnGround ? step : 0.02`), otherwise you cannot climb steps. Downhill movement on a ramp is
proportional to speed: `y − floorWalk <= max(step, horizontal × 1.5)`.

**Sanitize the rest of the abstraction:** `toAabbs() = List.of()`, `forAllBoxes/forAllEdges` empty,
`clip() = null`, `closestPointTo = Optional.empty()`, `getFaceShape = Shapes.empty()`,
`move/optimize/singleEncompassing = this`.

## 7.5 Make the block yield

```java
@Override
protected VoxelShape getCollisionShape(BlockState state, BlockGetter level,
                                       BlockPos pos, CollisionContext context) {
    return !state.getValue(HARD)
        && context instanceof EntityCollisionContext ec && ec.getEntity() != null
        && UkGeometry.collides(ec.getEntity())
        && UkGeometry.covers(pos)          // block AABB + 0.1 matches some tri
      ? Shapes.empty()                    // ← the actual geometry takes over
      : super.getCollisionShape(state, level, pos, context);
}
@Inject(method = "destroyBlock", at = @At("HEAD"), cancellable = true)
static void protect(ServerPlayerGameMode self, BlockPos pos, CallbackInfoReturnable<Boolean> cir) {
    if (WorldBridge.isHostBarrier(pos) || state.is(BARRIER) || state.is(UkSolid.BLOCK))
        cir.setReturnValue(false);        // build on it, but don't mine it
}
## 7.6 Push back (builds → host collision)

```
host: "blocks" {set:[x,y,z,...], clear:[x,y,z,...]}
      → GameObject with BoxCollider(size = Vector3.one * Map.S), isStatic
      → NavMeshObstacle { carving = true, carveOnlyStationary = true }   ← hence isStatic
```

⚠️ `carveOnlyStationary = true` **requires** `isStatic = true`. It’s the kind of line that costs you an hour of “why is the enemy walking through my wall?”

⚠️ **Smarter alternative (SkyCraft):** rewrite the **actor’s pathfinding**.
`PathAvoid::Install` hooks the path solver (`SetupPathHook::thunk`) and passes an `AvoidArray` of points
where movement is forbidden. Works with paths that ignore colliders (dynamic NavMesh, stuck detection, AI
that teleports when stuck).

## 7.7 Water (SkyCraft went further than everyone else)

**Rendering:** bend the fluid surface to sit over the actual terrain:
```java
if (fluidGround > 0.0F) {
    float t = Math.clamp(y - fluidBaseY, 0, 1);
    y = fluidBaseY + fluidGround + t * (1.0F - fluidGround);
}
```

**Physics:** 16×16 grid of surface heights, replacing the fluid at 4 points (`hasFluidAndLoaded`,
`getFluidState`, `FluidState.getHeight`, `getHeightForCamera`).

**Reverse direction:** MC water **refuses to flow** into host geometry:
```java
@Inject(method = "canPassThroughWall", at = @At("HEAD"), cancellable = true)
static boolean refuse(BlockGetter level, FluidState state, BlockGetter target, BlockPos pos,
                      Direction direction, FluidState targetState, CallbackInfoReturnable<Boolean> ci) {
    if (!targetState.isAir() || direction == Direction.UP) return;   // host only, air, not upward
    if (!SkyCollision.isKnown(pos)) { ci.setReturnValue(false); return; }   // ⚠️ unknown region
    if (direction == Direction.DOWN) {
        if (SkyCollision.hasGeometry(state, pos)) { ci.setReturnValue(false); return; }
        if (SkyCollision.groundTop(pos) >= 0.8388889) { ci.setReturnValue(false); return; }
    }
}
```

⚠️ **The first rule is the important one: unknown region → refuse.** Without this, water flows off the map
before you have geometry streaming.

---

# PHASE 8 — BIDIRECTIONAL COMBAT

## 8.1 Host → Minecraft

A prefix patch on a damage method:
```csharp
[HarmonyPatch(typeof(NewMovement), "GetHurt")]
static class HurtPatch {
    static bool Prefix(int damage, bool instablack, bool explosion) {
        if (!Plugin.Active) return true;
        if (instablack || damage >= 1000) return true;   // ⚠️ never steal a cutscene death
        Send(new { t = "hurt", d = damage * DamageScale });
        return false;
    }
}
```

## 8.2 Minecraft → Host

Send state, not collisions. SkyCraft sends `EV_HIT_ACTOR` with `HIT_CRITICAL/PROJECTILE/SWEEP/FIRE` flags and a weapon class (`BLADE/AXE/BLUNT/PIERCE/ARROW`), and the plugin
resolves the actual hit pipeline.

⚠️ **The proxy must be vulnerable with damage canceled:**
```java
public void tick() { baseTick(); setHealth(getMaxHealth()); }   // ⚠️ permanently full health
@Override protected void actuallyHurt(...) { pendingDamage += dmg; /* ... */ }
@Mixin(LivingEntity.class)
@Inject(method = "hurtServer", at = @At("HEAD"), cancellable = true)
static void proxyNeverDies(LivingEntity self, ServerLevel level, DamageSource src, float amt,
                           CallbackInfoReturnable<Boolean> cir) {
    if (MobWar.isProxy(self)) { MobWar.onProxyHit(self, src, amt); cir.setReturnValue(false); }
}
```
⚠️ If you make it **invincible**, no *target goal* can target it (invincible entities are untouchable), and mobs will simply ignore it.

⚠️ **The `Invisible` flag resets on the first network sync.** Give it an infinite, hidden `INVISIBILITY` effect.

⚠️ **Useful derivative:** SkyCraft trains **Skyrim skills** through Minecraft actions — blocking a
hit trains Block, smithing trains Smithing using `worth()` per material (netherite 150, diamond 80, iron
40…), armor is classified as heavy vs. light by the ID prefix. Guest content becoming host progression.

---

# PHASE 9 — BUILD ORDER

> **Build in this order.** Each step is independently verifiable, **each one produces something visible**, and
> each one has a **GATE** you must not pass until you see the result.
>
> ⚠️ The temptation to jump to step 5 (the interesting part) is exactly what causes the project to stall
> in week 2.

| # | Step | Deliverable | GATE — don’t proceed until |
|---|---|---|---|
| 1 | Empty link | `hb` every 0.1 s | see `connected to Minecraft at ws://127.0.0.1:25599` |
| 2 | Window overlay | host over MC | see the host on top, click, **and MC responds** |
| 3 | Test symbiosis | `{"t":"steve",pos,yaw}` → label | see the numbers move |
| 4 | Camera | pose in shm, texture, host camera | see the MC world through the host camera, responding to the mouse |
| 5 | **Depth** | depth-mesh / shader | MC **has depth**: host wall hides block, block hides host |
| 6 | Hide the character | `forceRenderingOff` + duplicate | see the character in the right place, or nothing |
| 7 | Block collision | simple grid | player stops walking through walls |
| 8 | Triangular geometry | `tris`/`trism` + SAT | smooth ramps, thin walls hold |
| 9 | Gravity and portals | `Map.Turn` + hold/release | pass through **with no perceptible pause** ← whichity metric |
| 10 | Combat | damage in both directions | take damage from both sides |
| 11 | Builds | `blocks` → colliders + navmesh | your walls stop enemies; enemies climb your stairs |
| 12 | Polish | style, sound, menus, LAN | — |

⚠️ **Estimate: 4–8 weeks at half time**, Layers 1–4 reusable with any other target. **If this is
your first modding project, double it.**

## Step 0 (new, before 1) — the fake oracles

⚠️ **Don’t start compositing anything before you have a fake host.** The GTA passthrough was
**mostly built before the 125 GB game finished installing**, using:

- `fakehost.py` — fake host with known geometry; composites MC frames over a synthetic scene
  + checkerboard + red pillar “only the host has.” The PNG shows whether alignment and occlusion are
  correct.
- `fakegta.cpp` — the entire GTA side **except the natives**: D3D11 window with a reversed-Z depth buffer
  and GTA camera conventions, with the compositor and WebSocket client compiled in.

This turns “I installed 125 GB and now I can test” into “run a 50 KB exe.” **Cost: 1–2 days. Payoff: removes
the bottleneck from step 5.**

## Step 5 in detail — this is the hard part

⚠️ **Choose β or γ in Phase 1 and don’t switch later.** If γ, the §0.6 depth test **has already been
done** — you’re here because it produced a gradient.

**β (depth-mesh):**
```
1/z = iz0 + iz1 × depth
vertex = (colX/iz, rowY/iz, S/iz)     ← camera space
```
The host’s perspective division reproduces the MC pixels exactly, so the **normal Z-test** handles
occlusion. Four pairs of coefficients come from `(flags & 1) × (flags & 4)`:

| flags | `iz0` | `iz1` |
|---|---|---|
| `&1` set, `&4` set | `1/far` | `(far−near)/(near·far)` |
| `&1` set, `&4` clear | `1/near` | `−(far−near)/(near·far)` |
| `&1` clear, `&4` set | `1/far` | `2(far−near)/(2·near·far)` |
| `&1` clear, `&4` clear | `1/near` | `−2(far−near)/(2·near·far)` |

Three steps per row band:
1. **Linearize** with a 3-tap filter (maximum of horizontal neighbors) — kills 1-pixel noise
2. **Dilate** the row by 1 and fill holes with the neighbors’ `hmax`
3. **Emit** 6 indices per quad, with a **discontinuity cull** `hi − lo > 0.15·hi + 0.01` → skip

⚠️ **Parallelize by row, never at a finer granularity.** The three passes are disjoint by construction — row
`i` only writes its row’s `invZ`/`verts`/`tris` — so `Parallel.For` over bands is safe without a
lock. Use `Bands = clamp(ProcessorCount − 2, 1, 8)`.
## Step 9 in detail — hold/release

The problem: when the surrounding collision needs to be rebuilt (entered a level, gravity changed,
crossed a portal), the player needs to stay still until the geometry exists — **otherwise they fall**.

**The solution: freeze Minecraft, not the player.**

```
host: hold on:true   → + geometry + grid; MC stays INVINCIBLE and locked
host: nearby grid ready (192 cells around)
host: hold on:false  → "collision ready around Steve (N cells): released after N ms"
```

```java
static void hold(LocalPlayer player) {
    if (released && HostState.attached()) { holdX = Double.NaN; Passthrough.playerHeld = false; }
    else {
        Passthrough.playerHeld = true;
        if (Double.isNaN(holdX)) { holdX = player.getX(); holdY = player.getY(); holdZ = player.getZ(); }
        if (holdY < player.level().getMinY() + 4) holdY = 100.0;   // safety net in the void
        player.setPos(holdX, holdY, holdZ);
        player.setDeltaMovement(Vec3.ZERO);
        player.fallDistance = 0.0;
    }
}
```

⚠️ **ack with a timeout on the host**, so it never hangs if one side dies:
```csharp
if (warpPending && (dist2 > 9.0 || now - warpSentAt > 1f)) { warpPending = false; }
```

**Teleport without rubber-banding:**
```java
player.setPos(to.x, to.y, to.z);
player.xo = player.xOld = to.x - vel.x;   // ← the INTERPOLATED position starts out on the correct side
player.yo = player.yOld = to.y - vel.y;
player.zo = player.zOld = to.z - vel.z;
player.setDeltaMovement(vel);
player.fallDistance = 0.0;
```

**Anti-embedding:**
```java
if (index().overlaps(box.deflate(1e-4))) {
    for (double d = 0.03125; d <= 2.000000001; d += 0.03125)          // steps of 1/32
        for (Vector3 dir : FOURTEEN_DIRECTIONS)                        // 6 axes + 4 diag + 4 + -y
            if (!level().noCollision(player, box.move(dir.scale(d)).deflate(1e-4))) {
                player.setPos(pos.add(dir.scale(d)));
                double into = v.dot(dir);
                if (into < 0.0) player.setDeltaMovement(v.subtract(dir.scale(into)));
                return;
            }
}
```
⚠️ Canceling the velocity component **in the direction of the wall** is what prevents the player from being
pushed out and back in alternately every frame.

---


## FINAL GATE — ACCEPTANCE / HANDOFF

Before declaring the project complete, produce a final report separate from MODLOG.md:

FINAL STATUS:
SUPPORTED GAME BUILD:
HOST:
GUEST:
ARCHITECTURE:
IPC:
RENDER:
DEPTH/OCCLUSION:
CAMERA:
COLLISION:
INTERACTION:
LIFECYCLE:
PERFORMANCE:
KNOWN LIMITATIONS:
RECOVERY PROCEDURE:
REPRODUCTION COMMAND:
ORACLES PASSED:
ORACLES FAILED:
UNVERIFIED CLAIMS:

### Acceptance criteria

- **🟢 VERIFIED:** all critical components in scope have been demonstrated in the target build.
- **🟡 PARTIAL:** the core works, but part of the scope remains incomplete.
- **🟠 PROTOTYPE:** the proof of concept works, but robustness/performance/lifecycle still do not
  meet the final scope.
- **🔴 BLOCKED:** a critical requirement cannot be implemented through the permitted route.
- **⚪ UNKNOWN:** cannot be used as a conclusion.

The agent **must not turn PROTOTYPE into VERIFIED just because the visual effect looks correct**.

---
# PHASE 10 — ORACLES (required for every step)

> Agents (and tired humans) fail at modding **by deriving**: building with conviction on a
> wrong guess. The remedy is an **oracle** — something mechanical that says right or wrong, run after
> every change.

## 10.1 The oracles

| Oracle | Catches | Real example |
|---|---|---|
| **Round trip** | misunderstood format | AoE2 SLD: decode→encode→decode, error 0.91/255 |
| **Trace replay** | a port that diverges from the game | Terraria EoC: state frame-t + action → compare t+1 (99.9%) |
| **Scripted scene + screenshot that you LOOK AT** | scale, orientation, pivot, layer | chat command that summons, screenshot |
| **Game log** | load error, exception, missing asset | see table below |
| **Synthetic host** | integration bug before the game exists | `fakehost.py`, `fakegta.cpp` |
| **Measurement scene** | timing and sync | MC-only golden wall vs host-only horizon |
| **Build byte-match** | decompilation error | decompilations that compile back to an identical ROM |
| **Publish check** | shipping what you shouldn't | `a publish check` |

**Logs:**

| Game / loader | Log |
|---|---|
| BepInEx | `BepInEx/LogOutput.log` |
| Unity | `Player.log` in `AppData/LocalLow/<company>/<product>/` |
| UE4SS | `UE4SS.log` |
| SKSE | `Documents/My Games/<game>/SKSE/` |
| tModLoader | `client.log` |
| **Minecraft** | `<gameDir>/logs/latest.log` |

## 10.2 The four rules

1. **Automate the entire loop** — launch → menus → scene → check → log — so that **one command**
   answers “did it work?”.
2. **Circuit breaker:** after ~3 identical failures, **stop**, write down what you know, change approach.
3. **A journal (`MODLOG.md`):** every confirmed fact, and every dead end **with the reason why**. Survives
   context compaction.
4. **Be honest about the result:** write down what the oracle **did not** cover.

## 10.3 ⚠️ The oracles that lie

**1. The frozen oracle.** A capture via `Windows.Graphics.Capture` **stops tracking** the window
as soon as the swapchain goes through ReShade/independent flip, and **replays the last composited frame** —
byte-identical, same SHA, even when the process is clearly rendering (7.2 s of CPU time per 5 s of wall
clock time).

> **What this reverses:** the oracle would say “the shader is not running” when the truth is “the shader runs
> perfectly and the depth it reads is empty.” This was literally the case with Wukong.

**How not to fall for it:** **prove the oracle is alive before trusting it.** Take two captures 1 s apart,
compare hashes; if they match while the game is animating, the oracle is dead. Use a capture from
**inside** what you are measuring (ReShade's `Print Screen` writes the post-processed frame next to the DLL).

⚠️ And the reverse: a legitimately static scene produces two identical captures. Check the process burning
CPU.

**2. Screenshots nobody looks at.** The agent saves them but never opens them. Open a reduced copy after
every visual change. It's the only way to catch “facing left instead of right.”

**3. Negative result without checking the loop.** A chunk scan reported “no string” because the loop
used `$exe.Length` on the **path string** (~100), not the file. Use
`(Get-Item $path).Length` and keep the loop.

**4. “Works on the fake host” ≠ “works in the game.”** The real game adds pausing, idle camera, window focus,
screensaver. Run the real game before saying you're done.

**5. Oracle testing the wrong frame.** Off-by-one: the action from frame t appears in t+1, and a script reads
the camera from the frame **being prepared**. Measure with a deliberate test before trusting the replay.

**6. `PYTHONUTF8=1`.** `um win ps` crashes on Windows with a non-UTF-8 locale (`UnicodeDecodeError: 'gbk'`)
because window titles contain bytes the locale cannot map. Before `um win ps/shot/drive`.

**7. DPI.** `um win drive click` receives **virtualized** coordinates; `um win shot` returns **physical**
pixels. At 150%: `click = screenshot_px / 1.5`.

---

### 10.4 Numerical oracles — the ones that don't lie based on opinion

⚠️ **“Looks right” is not an oracle.** These four give numbers, and numbers can be compared:

| Oracle | How | Acceptance |
|---|---|---|
| **Analytical projection** | an object at a known depth, projected by hand, vs. where it appears | **within ~1 px** |
| **Depth vs native raycast** | frame depth vs. the host's own raycast at the same point | **exact** (e.g., `5.5000` vs `5.5000` blocks) |
| **Measurement scene** | guest-only object against host-only background | moves together, no band |
| **Entity count** | how many things crossed, landed, took damage | matches the test script |

⚠️ **The fourth is what prevents false positives in multiplayer-like scenarios.** Documented in Portalcraft: the
logs showed *“entities carried through portals, landings (height, velocity,
damage), damage arriving at Portal 2 (`fall took 30.0 health`), heals, achievements, deaths to goo”*.
This is a list of verifiable claims, not an impression.

⚠️ And **test hooks require undo**: *“every test hook that changes the world has an
undo, and Survival campaign tests trigger real rewards: undo them.”* Otherwise the
user comes back to find their world changed.

# 11. CONSOLIDATED PITFALLS

All verified in decompiled code or in the binaries of the three implementations.
## 11.1 Architecture

1. **Nav shuts down when the frame turns.** `Nav.Rebuild`/`LinkBlocks` abort in `Map.Turned`, and
   `Nav.Near` uses `Quaternion.identity` even though the blocks are positioned with `Map.Frame` — safe
   *only because it never runs in that state*. Any refactor that removes `Map.Turned` will break it, and the cause
   will seem to have nothing to do with the navmesh.
2. **`ground` is dead code** in ULTRAKILL 0.2.0. Remove dead paths when porting between versions.
3. **GTA V inheritance.** The `gta`/`director` mthisges exist because ULTRAKILL is derived from that
   example. You don't need them — but if you change them, preserve the fan-out.
4. **Settings are captured in `Awake()`.** Changing `UnitsPerBlock` at runtime misaligns the geometry
   ("Steve floats 2 units"). Register a change handler or document that a restart is required.

## 11.2 Payload and threading

5. **`tris.d` is float32, not double.** Only `trism` is double (step 13).
6. **`List<double>` receiving an `int`.** `moves.Add(p.Id)` — it works, but it's a wrong type waiting
   to cause a bug. Cast explicitly.
7. **Vertices reassigned every frame.** `mesh.vertices = verts` every time, even when nothing has changed. Only the
   index is compacted. If the cost becomes noticeable, this is where it is.
8. **Stats that collapse.** `"moved most: name xN"` indexes by `Collider.name`; 300 pieces with the same
   name become one entry. Your profiling log lies.
9. **Binary ring wrap bug** (§3.6).
10. **Budget needs both a count AND a time limit.** SkyCraft uses `12 sections OR 3 ms`, whichever comes
    first. A count alone doesn't protect against a pathological section.

## 11.3 Engine

11. **ReShade needs to load as an ASI in some games.** GTA loads the system `dxgi.dll` before
    a proxy in its folder.
12. **Depth may be inaccessible.** Wukong/UE5/D3D12: hook, detect the depth-stencil, read
    **flat** depth. (§0.6, §10.3)
13. **Keys get stuck** when the host gains focus. `releaseAll()` on menu/loading/link loss.
14. **The guest auto-pauses** when it "loses" focus → the world is lost. Force `isFocused`/
    `isIconified`/`isPauseScreen`.
15. **`player_movement_check`** fights constant teleportation. Force `false`.
16. **Elytra: "gliding" is free fall.** With `noPhysics`, `onGround` never updates and the server cancels
    every glider. Force `setOnGround(false)`. Remove the elytra on join.
17. **Proxies remain visible.** Network sync resets `Invisible` with no effect. Infinite `INVISIBILITY`
    effect.
18. **Mobs ignore proxies.** Invincible = untouchable = not targetable. Leave them **vulnerable** and cancel the damage.
19. **⚠️ F3D3D `ReadBackbuffer` leaks** 17 GB in one take (Terraria/FNA). **Never** read the back buffer from
    inside the game. Capture the window from outside (ffmpeg `gfxcapture=hwnd=...`).
20. **Never block the main thread** waiting for ffmpeg to stop. Send `'q'` and wait off-thread.
21. **Write `.mkv` while recording:** survives a kill; mp4 loses the index.
22. **Paks/resources from the wrong version** crash on mount.
23. **Game updates move everything.** Pin the version and **document which build you support**.

## 11.4 Minecraft settings (that every passthrough overwrites)

The mod **must** force: no clouds, no view bobbing, no vignette, FOV effect 0, fps cap,
pause-off, mob spawning off, `time set noon`, `player_movement_check false`, `keep_inventory true`.

⚠️ This is **necthisry** and **destructive** to the default experience. **Required: separate game dir.**
All three do this (ULTRAKILL creates a void world; GTA a `passthrough`; SkyCraft a `mirror` preset).

## 11.5 Reprojection / timing

24. **One frame of lead.** The script reads the camera for the frame *being prepared*. Reproject to the
    **previous** pose. Measure with the golden wall scene.
25. **`GetTickCount` judder.** 16 ms steps. Use `nanoTime`/QPC.
26. **Internal resolution scale ≠ swapchain.** Wukong renders at 1708×1068 with a 2560×1600 swapchain.
    The composite has to match the **internal** resolution.

## 11.5b Compositing, input, and multiplayer — the most expensive ones

⚠️ All measured, all with the same pattern: **the symptom looks like a system issue, the cause is
a synchronization issue.**

32. **The on-screen camera is almost always one frame behind.** With a real user's settings, **70–80 %
   of frames** were drawn with the camera from the previous hook. See §5.8.
33. **The mob outline is cut off on one side while strafing.** The reprojection search started at the pixel
   "at infinity" and stopped where nothing matched — losing a band as wide as the parallax. A
   16-step march in 1/depth **still** jumped past ~6 blocks. See §5.8.3.
34. **The outline is cut off at the edges when turning.** Prediction error pushes the picture out of frame.
   Overscan of `tan(fov/2) × 1.08`.
35. **Swallowing `WM_*BUTTON*` doesn't block the button.** The engine receives the buttons through another path.
   ⚠️ *Clear the button flags in the input-generation hook* (`IN_ATTACK`/`IN_ATTACK2` in a
   `CreateMove`), not at the OS layer.
36. **Writing health directly breaks invulnerability frames.** ⚠️ *"Lava dealt damage 20 times
   per second — health was reset every tick, so Minecraft's i-frames never applied."*
   **Let vanilla compute the damage, send the health it took, mirror the health back.**
37. **Teleporting an entity makes it "slide."** `teleport` is interpolated by the client.
   **Recreate the entity at the destination** — as travel between dimensions does: restore from the backup,
   remove the old one **first** so the same UUID can be added again.
38. **`noPhysics` must be scoped, not permanent.** With it always on, the server collides the
   stand-in with barriers that the host player passes through (portals, gaps). ⚠️ *"`noPhysics` only
   while the server processes movement packets."*
39. **The key that opened the menu counts as a new press inside it.** ⚠️ *Ignore keys that were already
   held when a menu opened until they are released.* (This is the same case as SkyCraft's `RELEASE_ALL`,
   but more specific.)
40. **The mouse wheel doesn't scroll if you normalize twice.** The ReShade delta is **already in
   notches**. Dividing by 120 again zeroes it out.
41. **"No fall damage" has two causes.** The fall formula returns early for anyone who is flying
   (the stand-in always flies), **and** the gamerule was off. ⚠️ **Compute the guest's formula
   from the host's landing** (height + impact velocity, so slow glides don't count) and apply it with the correct
   fall source.
42. **The watchdog cannot use a frame heartbeat.** A minimized game produces no frames. ⚠️ *Check
   that the process exists.*
43. **Random username = reset everything.** New UUID on every launch. Pin `--username`.
44. **No hash doesn't mean "nothing to download."** A profile meta provided `sha1`/`size` for everything **except**
   the loader itself, and the installer interpreted the absence as "no need to download." ⚠️ *When there's no hash,
   download `<artifact url>.sha1` and verify it.*

### 11.5c Level geometry → voxel grid

45. **Voxelize only the surface shell.** When converting brushes to collision blocks, ⚠️ *only the
    surface shell becomes blocks* — the solid volume becomes nothing, and the interior isn't needed.
46. **Per-map offset for alignment.** ⚠️ *One shift per map puts most floors on
    exact block boundaries.* Without it, half the floors end up half a block above or below.
47. **Brushes don't cover everything: static props are missing.** Three layers:
    - **brushes/lumps** (the level geometry) → voxelize
    - **raycast trace** with a "hits nothing" filter → finds props and doors (floors by
      tracing downward, walls by point testing at mob height)
    - everything else → don't mirror, and document it
48. **Override the `dimension_type` so the world is tall enough.** The source level may
    have more vertical scale than the guest default.

## 11.6 Epistemology — the most important ones

27. **The frozen oracle.** (§10.3)
28. **Screenshot nobody looks at.** (§10.3)
29. **Negative result without checking the loop.** (§10.3)
30. **"Works in the fake" ≠ "works in the real thing."** (§10.3)
31. ⚠️ **Most important:** the Wukong alley was found **deliberately, before** writing
    any compositing code. Depth reconnaissance was *front-loaded*. If you're going to use γ, that's the
    **first** task, not the last.

---

# 12. PER-GAME CHECKLIST (fillable)

> Copy, fill it out for the game in question, and keep it in `MODLOG.md`.

## Recon
- [ ] Game: `<version/build>` — Steam appid `<id>`
- [ ] Engine: `<engine>` `<version>` — loader: `<which>` `<version>`
- [ ] Anti-cheat: `<which>` — test mode: `<how>`
- [ ] Logs: host `<path>` · MC `<gameDir>/logs/latest.log`
- [ ] Saves: `<folder>` — backup made: `<when>`
- [ ] A1 depth: `<answer + evidence>`
- [ ] A2 camera: `<answer>`
- [ ] A3 collision: `<resource/format>`
- [ ] A4 damage: `<method>`
- [ ] Depth gate (if γ): `<measured result>`
- [ ] Safety gate (§0.4): passed / **stopped**
## Architecture
- [ ] Camera: `<A|B|C>` — `<reason>`
- [ ] Rendering: `<α|β|γ|δ>` — `<reason>`
- [ ] Transport: `<JSON+shm | binary shm | shm+LAN>`
- [ ] `UNITS_PER_BLOCK = <measured>`
- [ ] Angle table: `<derived>`
- [ ] Applicable playbook §2: `<which engine>`

## Layers
- [ ] Link + heartbeat + PID in shm
- [ ] ABI + ring of 3 + 3 fences + watchdog
- [ ] Overlay/window injection
- [ ] Protocol + tables
- [ ] Seqlocks / rings
- [ ] Affine `Map` + `Version`
- [ ] Collision: 13-axis SAT + Sutherland–Hodgman + yielding block
- [ ] Combat in both directions

## The 12 steps (with each step's gate)
1. [ ] Link — `GATE:` see `connected to Minecraft`
2. [ ] Overlay — `GATE:` clicking responds in MC
3. [ ] Symbiosis — `GATE:` numbers move
4. [ ] Camera — `GATE:` camera follows the mouse
5. [ ] Depth — `GATE:` correct occlusion
6. [ ] Character — `GATE:` character in the right place
7. [ ] Block collision — `GATE:` does not pass through walls
8. [ ] Triangles — `GATE:` smooth ramp
9. [ ] Gravity/portal — `GATE:` **no perceptible pause**
10. [ ] Combat — `GATE:` damage in both directions
11. [ ] Builds — `GATE:` enemies climb their stairs
12. [ ] Polish

## Reprojection (if using γ or δ with predicted camera)
- [ ] Read the view-projection from the **shader constants**, not just the engine hook
- [ ] Predictor with **EMA** of `shown - rendered_for`
- [ ] Two distinct cameras: predicted (for the guest) and on-screen (for reprojection)
- [ ] Coarse tile pass (1/16) → **min-depth**
- [ ] Near-to-far pass at ~1 px, with thickness test
- [ ] **Overscan** `tan(fov/2) × 1.08`; bilinear color, point-sampled depth
- [ ] Input raycast from the **camera**, not the eye (third person)

## Numerical oracles (required)
- [ ] Analytical vs. observed projection: **within ~1 px**
- [ ] Frame depth == native raycast: **exact**
- [ ] Entity/landing/damage counts match the script
- [ ] Test hooks with **undo** (including Survival rewards)

## Lifecycle
- [ ] Launcher: proxy → hidden guest → host → save/exit → **remove the proxy**
- [ ] Watchdog checks **the process**, not the frame heartbeat
- [ ] Fixed `--username`
- [ ] `cheats`/vac/etc. side effects documented for the user

## Proxy DLL
- [ ] `dumpbin /imports` to see **which folder** the host loads from
- [ ] Proxy in the correct **subfolder** (`bin`, `game/bin`)
- [ ] **Proof that it loaded** (log in `DllMain` or marker file)
- [ ] Tthisd on a clean install

## Damage and i-frames
- [ ] Let vanilla calculate damage, **do not** write health directly
- [ ] Fall damage recomputed from host physics (height + impact velocity)
- [ ] `noPhysics` only in the right scope

## Oracles
- [ ] Synthetic host exists (before step 5)
- [ ] Screenshot oracle **proven alive** (different hashes 1 s apart)
- [ ] Latency measurement scene (gold wall)
- [ ] `MODLOG.md` with facts **and dead ends with the reasons**
- [ ] Circuit breaker defined (3 failures → stop)

## Delivery
- [ ] Game dir separate from MC
- [ ] `Uninstall` written
- [ ] `um publish check` passed
- [ ] Loader/license credits
- [ ] **Honest AI disclosure**

---


## APPENDIX E — ANTI-REGRESSION FOR AGENTS

Use this checklist before each significant commit:

- [ ] I read the Universal Modder knowledge for the game/engine when available.
- [ ] I confirmed the current version/build.
- [ ] I did not copy offsets/signatures/addresses from another build without validation.
- [ ] HOST/GUEST is explicit.
- [ ] The chosen rendering path has evidence.
- [ ] The IPC has an oracle.
- [ ] The current milestone has an oracle.
- [ ] I am not using UNKNOWN as PASS.
- [ ] I am not repeating an attempt without producing new evidence.
- [ ] Rollback is documented.
- [ ] MODLOG.md records facts, negatives, and hypotheses.
- [ ] The scope of the current milestone is frozen.
- [ ] Game-specific code is separated from reusable infrastructure.
- [ ] No game files have been committed to the repository.
- [ ] The final result is classified as VERIFIED, PARTIAL, PROTOTYPE, or BLOCKED.

**Principle:** a reliable agent is not one that always says "I did it." It is one that knows exactly
what it has managed to prove.

---

# APPENDIX A — REFERENCE NUMBERS

⚠️ **These are not interchangeable. Measure your own.**

## ULTRAKILL

| Constant | Value |
|---|---|
| WebSocket port | `25599` |
| shm name / magic | `Local\MCPassthroughFrame` / `1414546253` (`0x5450434D`) |
| shm size | `298 602 496` (284 MB) |
| header / slots / stride | `4096` / `3` / `99 532 800` |
| maximum frame | `3840 × 2160` (`33 177 600` B/plane) |
| slot watchdog | `1 s` |
| **host units per block** | **2** |
| Steve scale | `0.95` (1.71 blocks, below V1's 1.75) |
| step height | `1` (vanilla is 0.6) |
| block grid | XZ ±14, Y −8..+12 = 17 661 cells |
| "ready" cells | radius 3.5 → **192** |
| block budget/frame | `2000` (≈9 frames per scan) |
| geometry radius | `40` blocks, scan every `0,25 s` |
| colliders per scan | `Collider[8192]` |
| depth steps/px | `3` → ~1.3 M triangles at 1080p |
| thread bands | `clamp(cores − 2, 1, 8)` |
| penetration / SAT epsilon | `0,02` / `1,0E-7` (degenerate axis `1,0E-12`) |
| broad-phase cell | `16` blocks (world), `(hi−lo)/24` (per piece) |
| **bytes per triangle** | **36** (9 × float32) |
| **bytes per moved piece** | **104** (13 × double) |
| host→MC / MC→host damage | `0,2` / `0,15` |

## SkyCraft

| Constant | Value |
|---|---|
| shm name / magic / version | `Local\SkyCraft_v1` / `1129925459` / `10` |
| shm size | `200 327 168` (~191 MB) |
| **SKSE units per block** | **70** |
| SKY_STATE / MC_STATE | `256` / `512` |
| WATER_GRID / OVERLAY_CTL | `1024` / `768` |
| INPUT_RING / ACTOR_TABLE / EVENT_RING | `4096` / `73 728` / `94 208` |
| WORLD_ENTITIES / COLLISION_RING | `114 688` / `131 072` |
| OVERLAY_PIXELS / RENDER_RING | `33 685 504` / `133 218 304` |
| input ring / event ring | 4096 × 16 B / 512 × 32 B |
| actor table / world entities | 256 × 64 B / 160 × 96 B |
| collision ring / render ring | 32 MB / 64 MB |
| **vertex** | **32 B** (x,y,z, u,v, rgba, light, flags) |
| collision per region / triangle | 8³ shape, header 32 B, block 80 B / tri 40 B |
| `WALKABLE_NY` / `FLOOR_SAMPLES` | `0,7` / `13` |
| SKYRIM→MC damage | `÷ 5,0` |
| mixins | 37 classes, 51 injection points |
| `REN_*` commands | 11 |
| mirror world | min_y −1024, height 2048 |
| multiplayer | up to 100 (LAN via e4mc) |

## GTA V

| Constant | Value |
|---|---|
| host | ScriptHookV ASI + ReShade 6.8 |
| GTA depth | **reversed-Z** |
| mapping | 1 meter = 1 block; (x,y,z) → (x, z+off, −y) |
| angles | MC yaw = 180 − heading; pitch = −pitch |
| script objects | max ~400 (crashes at ~1500) |

---

# APPENDIX B — THE THREE WAYS TO PROVIDE DEPTH

| | α overlay | β depth-mesh | γ shader | δ guest renders |
|---|---|---|---|---|
| **Time** | hours | ~2 weeks | ~2 weeks | ~1–2 weeks |
| **Occlusion** | no | **yes** | **yes** | **yes** |
| **Host lighting on guest** | no | yes (vertices) | **yes (shader)** | **yes (shader)** |
| **Relighting** | no | approximate | **yes** | **yes** |
| **Depends on the host's internal pipeline** | no | no | **yes** | **yes** |
| **Requires host depth** | no | **yes** | **yes** | **no** |
| **Minecraft can run slowly** | yes | yes | yes | **doesn't matter** |
| **Reimplemented in** | ULTRAKILL | ULTRAKILL | GTA | SkyCraft |

⚠️ **The tradeoff:** if you don't have reliable access to host depth, β and γ are out. **δ does not
require depth** — only the matrices, to place geometry in the right location. So δ is the plan,
not the backup plan.

---


## The three passthroughs

| | Source | Notes |
|---|---|---|
| **MinecraftInsideULTRAKILL 0.2.0** | release `0.2.0` (chavi) | `MinecraftPassthrough.dll` (BepInEx 5 + Harmony) + Fabric jar. Derived from the GTA example. |
| **Minecraft inside GTA V** | `universal-modder` → `examples/minecraft-gta5-passthrough` | `gta/src/script.cpp`, `gta/shaders/MCPassthrough.fx`, `mc/`, `host/fakehost.py`, `gta/tests/fakegta.cpp`. Notes in `knowledge/games/gta-v/minecraft-passthrough.md` (**the 21 lessons**). |
| **SkyCraft 0.1.0** | release `0.1.0` (chasmlol) | `SkyCraft.dll` (SKSE, CommonLibSSE-NG, DX11, Havok) + Fabric jar + bundled Prism Launcher. |
## The knowledge base

`github.com/rehan-remade/universal-modder` — 16 game notes, 4 technique notes, 12 engine playbooks.

Most relevant:
- `knowledge/games/gta-v/minecraft-passthrough.md` — **the 21 lessons.** Lessons 1, 8, 9, 10, and 20 are more costly.
- `knowledge/games/black-myth-wukong/reshade-depth-dead-end.md` — the **negative result** for
  depth. Read it before choosing γ.
- `knowledge/techniques/oracles-how-agents-know-a-mod-works.md` — the 8 oracles and the frozen oracle.
- `knowledge/techniques/driving-real-games-safely.md` — never automate clicks on the login screen.
- `skills/mashup-mods/SKILL.md` — the mashup workflow; this playbook covers **only** the
  passthrough pattern (two live processes).
- `skills/mod-any-game/references/engines/*.md` — one per engine.
- `skills/mod-any-game/references/safety.md` — the rules and why they exist.
- `skills/mod-any-game/references/case-studies.md` — Terraria, AoE2, GTA, the mashup wave.

## Tools

| Need | |
|---|---|
| Toolkit CLI | `um scan <game>`, `um win shot/drive/record`, `um backup`, `um publish check`, `um kb search` |
| Managed decompilation | `ilspycmd -p -o ~/<game>-decomp Assembly-CSharp.dll`, dnSpyEx |
| IL2CPP decompilation | Cpp2IL, Il2CppDumper (+ Ghidra/IDA for native bodies) |
| Java decompilation | Vineflower, CFR, Procyon, Recaf |
| Native decompilation | Ghidra (MCP: GhidraMCP, pyghidra-mcp), IDA (Hex-Rays MCP), ReVa |
| Graphics | RenderDoc (+ renderdoc-mcp) |
| Live | Cheat Engine, x64dbg, Frida, UnityExplorer, UE4SS Live View, REFramework-MCP |
| ⚠️ | **Try extracting strings first.** The SkyCraft build preserved RTTI and revealed the entire architecture without a decompiler. |

## Minecraft

- Modrinth / Fabric — Minecraft's most active network
- Mixin + **MixinExtras** (`@WrapOperation` comes from here). ⚠️ SkyCraft uses `@WrapOperation` in 13
  mixins and **does not declare MixinExtras in `depends`** — it is transitive. If you copy it, declare it.

## Licenses

| | |
|---|---|
| MIT | BepInEx, Harmony, Fabric, Fabric API, Java-WebSocket, CommonLibSSE-NG, spdlog, {fmt}, xbyak, SimpleIni, the three reference projects |
| BSD-3 | xbyak |
| Apache-2.0 | Fabric API |
| ⚠️ **GPL-3.0** | **Prism Launcher** — if you embed the launcher, the bundle is subject to the GPL |
| No redistribution | ScriptHookV, ReShade (the example only automates the download) |

⚠️ **Disclosure:** the three reference projects disclose the use of AI. ULTRAKILL is "written with Claude
Code and the universal-modder plugin, directed and play-tthisd by chavi". GTA is "Written with Claude
Code", inspired by chasm's Minecraft-in-Skyrim passthrough and TobynJacobs' Minecraft-in-Elden-Ring. **Be
honest about what is AI-generated** — communities react poorly to undisclosed "vibe-coded" releases,
and some recomp Discords **ban** AI projects.

---

*Built from: `ilspycmd` on `MinecraftPassthrough.dll`; `vineflower` on the ULTRAKILL jar and the
SkyCraft jar; the field note `portalcraft-minecraft-inside-portal-2.md`
(Preface: **predicted camera + near-to-far reprojection**, §0.6b, §2.15, §2.16, §5.8, §10.4, §11.5b/c); string extraction + RTTI on `SkyCraft.dll` (3.6 MB, PE32+ x86-64,
MSVC RelWithDebInfo, 9 sections, RTTI preserved); the `universal-modder` knowledge base; and the
source code for the `minecraft-gta5-passthrough` example.*

*⚠️ **What was NOT verified:** nothing was run. None of the three engines was installed. ✅ =
reading decompiled code and binary symbols. 📚 = general engineering knowledge. The instructions in §2 are
**contracts to be filled in**, not tthisd code — adapting them to the specific game is your job, and that work isn't covered here.*
---
# END ORIGINAL HARDENED PLAYBOOK CONTENT




