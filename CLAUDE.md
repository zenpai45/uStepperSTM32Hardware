# uStepperSTM32Hardware — Arduino BSP

Arduino core (board support package) for uStepper STM32 boards. **Fork of `stm32duino/Arduino_Core_STM32`**; follows the Arduino platform specification. Current target: uStepper S32 on STM32F401xC (Cortex-M4F).

Planning, roadmap, and multi-repo coordination live in `../ustepperSTM32.plan` (GitHub: `zenpai45/ustepperSTM32.plan`). Issues in this repo are children of planning epics — title prefix `[<epic-slug>]`, body references the parent.

## Structure

- `boards.txt` — board menu entries, per-variant compile/upload flags
- `platform.txt` — toolchain definition, compile and upload recipes
- `variants/STM32F4xx/<variant>/` — pin map (`variant_<BOARD>.{h,cpp}`, `PeripheralPins.c`, `PinNamesVar.h`) per board
- `cores/arduino/` — Arduino runtime (upstream stm32duino; avoid diverging unless necessary)
- `libraries/` — helper libs bundled with the core (`ModbusMaster`, `modbus`, `uStepperGcode`, `uStepperRobotArm`, `EEPROM`, `IWatchdog`, `Servo`, `SoftwareSerial`, etc.); each has its own `library.properties` + `src/`
- `system/` — CMSIS + HAL sources (upstream ST / stm32duino)
- `package.json` — Boards Manager index; consumers pull this via "Additional Boards Manager URLs"
- `VERSION` — plain version string, bumped on release
- `CI/` — compile-test scaffolding referenced by the workflow
- `.github/workflows/ustepper-compile.yml` — CI; must stay green

## Conventions

- **Stay close to stm32duino upstream** in `cores/`, `system/`, and the `variants/` scaffolding. Divergence = painful merges forever. When a patch is unavoidable, isolate it and document *why* in the commit message.
- **One variant = one subdirectory** under `variants/STM32F4xx/`. The pin map and peripheral pins are the contract between this BSP and the library.
- **Bundled libraries** under `libraries/` each follow the Arduino library spec (own `library.properties` + `src/`). Treat them as sub-projects with their own versioning.
- **Upload path** for uStepper S32 is DFU via STM32CubeProgrammer — not ST-Link, not SWD by default. Entry is the physical `boot`/`reset` button dance.

## Adding a new board variant

1. Create `variants/STM32F4xx/<variant>/` with `variant_<BOARD>.h`, `variant_<BOARD>.cpp`, `PeripheralPins.c`, `PinNamesVar.h`.
2. Add a `uStepperSTM32.menu.pnum.<BOARD>=...` block in `boards.txt` (copy the `USTEPPER_S32` block as a template; swap MCU-specific fields).
3. Bump `VERSION` and the version + archive entry in `package.json`.
4. Extend `.github/workflows/ustepper-compile.yml` to compile-test an example against the new board.
5. If the new board has a different MCU series (not STM32F4), split/condition `uStepperSTM32.build.series` / `mcu` / `flags.fp` accordingly.

## Syncing from upstream stm32duino

Periodic upstream sync is expected but is **not routine — it's a planned epic**, not a background task. Before syncing: snapshot uStepper-specific diffs (grep `uStepper` / `USTEPPER` / `UStepper`), merge, re-apply where conflicted, then run the full CI matrix.

## Before finishing a refactor

If top-level structure changes (a top-level directory added/removed, the boundary between `cores` / `variants` / `libraries` / `system` shifts, or the `boards.txt` / `platform.txt` / `package.json` triad changes shape) **update the sibling cheat sheet in `../ustepperSTM32.plan/CLAUDE.md` in the same PR**. The cheat sheet is deliberately coarse (top-level buckets only); reorganisation below that level does not require a sync.
