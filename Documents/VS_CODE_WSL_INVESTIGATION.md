# VS Code WSL2 Menu Investigation

## Issue Summary
VS Code development build crashes in WSL2 environment when interacting with menus. Crash occurs after:
- Menu handling (`menubarService#updateMenubar`)
- File operations related to settings.json
- Error resolving tasks.json

## Investigation Steps Taken
1. Attempted running with modified flags:
   - `--no-native-window-menu`
   - `--disable-gpu-compositing`
   - Various Electron debug flags

2. Code analysis:
   - Identified key files: `menubarControl.ts`, `menubarMainService.ts`
   - Found interaction between main process and renderer process for menu handling

## Next Steps
1. Test in native Windows environment to isolate WSL2-specific issues:
   - Clone and build in Windows
   - Test menu functionality
   - Compare behavior with WSL2

2. Consider Linux testing options:
   - Local VM
   - GitHub Codespaces
   - Compare behavior across platforms

## Key Files
- `src/vs/workbench/electron-sandbox/parts/titlebar/menubarControl.ts`
- `src/vs/platform/menubar/electron-main/menubarMainService.ts`
- `src/vs/platform/menubar/common/menubar.ts`

## Build Commands
```bash
# WSL2 Test Command
ELECTRON_ENABLE_LOGGING=true \
ELECTRON_ENABLE_STACK_DUMPING=true \
ELECTRON_DISABLE_GPU=true \
ELECTRON_DEBUG_NATIVE_WINDOWS=1 \
ELECTRON_ENABLE_DETAILED_LOGGING=1 \
VSCODE_DEV=1 \
./scripts/code.sh \
--disable-gpu-compositing \
--force-renderer-accessibility \
--disable-native-titlebar