<!-- Copilot instructions for the led-blink ESP-IDF example -->
# Copilot instructions — led-blink

These instructions are tailored for automated coding agents working on the `led-blink` ESP-IDF example in this repository. Keep guidance short and focused on discoverable patterns.

- Project type: ESP-IDF C project using CMake. Entry point: `app_main()` in `main/main.c`.
- Build system: CMake + `idf.py`. The root `CMakeLists.txt` includes `$IDF_PATH` helper and sets `MINIMAL_BUILD ON`.

Quick workflows (what to run):

- Build: `idf.py build` (from repository root). Requires `IDF_PATH` and ESP-IDF toolchain.
<!-- Copilot instructions for the led-blink ESP-IDF example -->
# Copilot instructions — led-blink

These instructions are tailored for automated coding agents working on the `led-blink` ESP-IDF example in this repository. Keep guidance short and focused on discoverable patterns.

- Project type: ESP-IDF C project using CMake. Entry point: `app_main()` in `main/main.c`.
- Build system: CMake + `idf.py`. The root `CMakeLists.txt` includes `$IDF_PATH` helper and sets `MINIMAL_BUILD ON`.

Quick workflows (what to run):

- Build: `idf.py build` (from repository root). Requires `IDF_PATH` and ESP-IDF toolchain.
- Flash: `idf.py -p <PORT> flash`.
- Monitor: `idf.py -p <PORT> monitor`.

Repository-specific patterns and notes:

- Minimal example: `idf_build_set_property(MINIMAL_BUILD ON)` in the root `CMakeLists.txt` — only essential components will be available. Avoid adding large optional components.
- Single-component layout: `main/CMakeLists.txt` uses `idf_component_register(SRCS "main.c" INCLUDE_DIRS ".")` — follow this pattern when adding sources.
-- Hardware constants: `main/main.c` sets the LED pin to 13 and the blink time to 2 (look for the LED_PIN and BLINK_TIME defines). If you need configurability, prefer adding a Kconfig entry and exposing it via `sdkconfig` rather than hard-coding.
- FreeRTOS timing: use `vTaskDelay(ms / portTICK_PERIOD_MS)` for delays (this project uses that pattern).

Files to inspect when making changes:

- `main/main.c` — GPIO setup and main task loop.
- `main/CMakeLists.txt` — how the component is registered.
- `CMakeLists.txt` (root) — project bootstrap and minimal build hint.
- `pytest_hello_world.py` — example automated test harness (CI-friendly pattern).

Coding style and edits guidance:

- Preserve existing C style and ESP-IDF API usage (gpio_reset_pin, gpio_set_direction, gpio_set_level).
- When updating build configuration keep `idf_component_register` and avoid adding global variables at the root scope.
- Prefer `menuconfig`/`Kconfig` for new configuration options. If you add Kconfig, update `sdkconfig.defaults` or provide documentation.

Integration and dependencies:

- This project requires the ESP-IDF SDK + toolchain. Do not replace the build system with non-ESP-IDF tooling.
- Serial flashing/monitoring relies on `idf.py` and a connected serial port.

When proposing patches, include:

- One-line rationale for the change and why it is safe for a minimal example.
- Exact files changed (path and short description).
- A simple test: "Build and flash with `idf.py`; verify LED blinks at the expected interval."

If anything above is unclear or you want more examples from this repo, ask and point to the file you want examined.
