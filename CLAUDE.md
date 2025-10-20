# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Shuttle is a macOS native status bar application built with Objective-C and Cocoa that provides quick SSH connections and terminal commands through a menu bar interface. It integrates with both Terminal.app and iTerm2.

## Architecture

### Technology Stack
- **Objective-C** with **Cocoa Framework**
- **Xcode** project structure (`Shuttle.xcodeproj`)
- **AppleScript** for terminal integration
- **JSON** configuration files
- **Multi-language support**: English, Spanish, French, Chinese (Simplified)

### Key Components
- **AppDelegate** (`Shuttle/AppDelegate.h/m`): Main application delegate handling menu creation and system integration
- **AboutWindowController** (`Shuttle/AboutWindowController.h/m`): About dialog management
- **LaunchAtLoginController** (`Shuttle/LaunchAtLoginController.h/m`): macOS login item management
- **Terminal Integration**: Compiled AppleScript files in `apple-scpt/` directory
- **Configuration**: JSON-based configuration (`~/.shuttle.json`)

## Development Commands

### Build & Development
```bash
# Build the Xcode project
xcodebuild -project Shuttle.xcodeproj -configuration Debug

# Build for release
xcodebuild -project Shuttle.xcodeproj -configuration Release

# Clean build
xcodebuild clean -project Shuttle.xcodeproj
```

### AppleScript Compilation
```bash
# Compile terminal integration scripts
cd apple-scripts
./compile-iTermStable.sh     # For iTerm2 stable
cd apple-scripts
./compile-iTermNightly.sh   # For iTerm2 nightly
cd apple-scripts
./compile-Terminal.sh       # For macOS Terminal
cd apple-scripts
./compile-Virtual.sh        # For virtual terminal with screen
```

### Configuration Testing
```bash
# Validate JSON configuration
cat ~/.shuttle.json | python -m json.tool

# Test with sample configurations
cp tests/shuttle.json.sample ~/.shuttle.json
```

### Development Workflow
1. **Open**: `open Shuttle.xcodeproj` (opens in Xcode)
2. **Build**: Use Xcode GUI or `xcodebuild` commands above
3. **Test**: Run built application from Xcode or `/Applications/Shuttle.app`
4. **Debug**: Check Console.app for Shuttle logs

## File Structure

```
Shuttle/                    # Main source directory
├── Shuttle.xcodeproj/      # Xcode project files
├── Shuttle/               # Application source
│   ├── AppDelegate.h/m    # Main application logic
│   ├── AboutWindowController.h/m
│   ├── LaunchAtLoginController.h/m
│   ├── main.m            # Entry point
│   └── shuttle.default.json # Default config template
├── apple-scripts/         # Source AppleScript files
├── apple-scpt/           # Compiled AppleScript (.scpt files)
├── tests/                # Test configurations
└── *.lproj/              # Localization files
```

## Configuration Format

The application uses JSON configuration (`~/.shuttle.json`) with structure:
- **terminal**: Terminal type ("Terminal.app" or "iTerm")
- **launch_at_login**: Boolean for startup behavior
- **show_ssh_config_hosts**: Boolean to parse ~/.ssh/config
- **hosts**: Array of host configurations with commands
- **menu nesting**: Supports hierarchical menu structure

## Testing & Validation

### Manual Testing
1. Build application
2. Copy to `/Applications/`
3. Create/modify `~/.shuttle.json`
4. Test menu functionality
5. Verify terminal integration works

### Configuration Testing
- Use test files in `/tests/` directory
- Validate JSON syntax before deployment
- Test with various terminal applications

## Special Considerations

### Permissions
- Requires **NSAppleEventsUsageDescription** for AppleScript execution
- Uses macOS app sandboxing
- Requires Accessibility permissions for terminal automation

### Localization
- Uses `.lproj` directories for internationalization
- Localized strings in `*.strings` files
- Interface files in `*.xib` format

### Terminal Integration
- Supports Terminal.app, iTerm2 (stable/nightly/legacy)
- Uses AppleScript for terminal automation
- Handles SSH connections and custom commands
- Supports window/tab management preferences