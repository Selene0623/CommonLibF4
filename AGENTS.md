# CommonLibF4

Fallout 4 F4SE plugin development library. Windows-only (MSVC).

## Build

**Prerequisites:** VS2019/2022 with C++ workload, vcpkg, CMake 3.21+, clang-format 12.0.0.

```bash
cmake --preset vs2022-windows-vcpkg   # or vs2019-windows-vcpkg
cmake --build build --config Debug
ctest --test-dir build -V
```

In-source builds are forbidden. Build output goes to `build/`.

## Repository Structure

| Directory | Type | Description |
|---|---|---|
| `CommonLibF4/` | Static lib | Core library (RE, REL, F4SE namespaces). Public headers in `include/`. |
| `ExampleProject/` | Shared lib | F4SE plugin example/template. DLL output. |
| `F4SEStub/` | Executables | `loader/`, `runtime/`, `steam_loader/` subdirs. |
| `Tests/` | Executable | Catch2 tests. Depends on CommonLibF4. |
| `RTTIDump/` | Shared lib | RTTI dump tool. Depends on CommonLibF4. |
| `AddressLibGen/` | Executable | Address library generator. |
| `AddressLibDecoder/` | Executable | Address library decoder. |

## Key Facts

- **C++20 required**, enforced via `CXX_STANDARD 20` in CMake.
- **Static linking**: vcpkg triplet `x64-windows-static-md`, MSVC runtime `MultiThreaded$<$<CONFIG:Debug>:Debug>DLL`.
- **Warnings are errors** (`/WX`).
- **Precompiled headers**: `include/F4SE/Impl/PCH.h` (CommonLibF4), `src/PCH.h` (other targets).
- **Formatting**: clang-format 12.0.0 with custom `.clang-format` (4-space tabs for C++/CMake, 2-space for JSON).
- **CI**: GitHub Actions, `windows-latest`, preset `vs2022-windows-vcpkg`, Debug build.
- **Dependencies managed via vcpkg**: see root `vcpkg.json` (args, boost-stl-interfaces, catch2, fmt, frozen, nowide, robin-hood-hashing, rsm-mmio, spdlog, srell, xbyak).
- **Copy build option**: CMake option `COPY_BUILD` copies output to `$Fallout4Path/Data/F4SE/Plugins/` when set.
