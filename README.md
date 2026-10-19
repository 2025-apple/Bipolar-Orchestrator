# Bipolar Orchestrator (Eco-accommodated copy)

The Bipolar Orchestrator and its GUI were made by **Valentina Buitron** ([@TheFIXmess](https://github.com/TheFIXmess)) and **Viviana Fraccaroli** ([@vivis2kuni](https://github.com/vivis2kuni)). The GUI is Valentina's individual contribution; see [`gui/README.md`](gui/README.md) for its concept, controls and credits.

This copy was edited by **Trisha Agrawal** only so that her Explorer **Eco** ([explorer_AP_UNITN-2025](https://github.com/luna4-ap2025/explorer_AP_UNITN-2025)) runs on this orchestrator and shows up in the GUI. It exists for demonstration purposes. The Luna 4 orchestrator has no GUI, so this is the setup used to show Eco running with a visualizer.

All credit for the orchestrator and the GUI belongs to their authors. The edits listed below are the only changes.

## What was changed

| File | Change |
|------|--------|
| `orchestrator/Cargo.toml` | Added `explorer_eco` (git dependency); removed the Jebediah explorer dependency use from the builder. |
| `orchestrator/src/builder.rs`, `orchestrator/src/main.rs` | Eco is spawned in the slot Jebediah had (**explorer id 2, starting on planet 1**) in both the GUI and the CLI setup. Viviana is unchanged (id 1, planet 4). |
| `orchestrator/src/api/mod.rs` | Before the game starts, each explorer's reply channel is given to its planet with an `IncomingExplorerRequest` (the same handshake a normal move uses). A planet only knows where to answer an explorer after this message. Also holds Eco's status cell (`eco_status`). |
| `orchestrator/src/snapshot.rs`, `shared/src/lib.rs` | New `EcoPanel` (day, regime, coins, earned, spent, task, decision, waiting-for-income). `snapshot::build` fills it from Eco's status. |
| `gui/src/main.rs` | Explorer 2 is displayed as "eco" and its hologram shows the `EcoPanel` text above the bag. The bridge thread also keeps GUI events that were not yet read, so none are lost between snapshots. |

A short log of the same changes is in `orchestrator/changes.txt`.

## How Eco is connected

Eco only uses the shared protocol channels from `common-game`. It does not depend on this orchestrator or on the GUI. The status panel is an optional extra: the orchestrator passes Eco a `StatusSink` (a shared cell that Eco writes its latest status into), and the GUI reads it about four times a second. Without a sink, Eco runs the same.

## Running it

Run these from the repository root.

```bash
# GUI (Bevy window)
cargo run -p bipolar-gui

# Command-line orchestrator
cargo run -p bipolar-orchestrator
```

The first build downloads all the planet and explorer crates from GitHub and compiles Bevy, so it takes a while.

## Known limits

- The GUI is fixed to 1920x1080, 7 planets and two explorers (see `gui/README.md`).
- Jebediah's sprites remain in the assets but he is no longer spawned.
