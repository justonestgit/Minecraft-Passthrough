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
workflow, bridge-contract, playtest and attribution templates.
