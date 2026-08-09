# Repository Guidelines

## Project Structure & Module Organization

- `App/` contains the iOS/Catalyst application, including scene delegates, controllers, SwiftUI views, keyboard UI, and app configuration.
- `Common/` contains the shared framework: terminal controllers, preferences, models, extensions, and VT100 helpers.
- `NewTermLoginHelper/` contains the C helper used to launch terminal processes.
- `Resources/`, `Fonts/`, and `assets/` hold localization, themes, media, bundled fonts, and repository artwork.
- `Deps/` vendors SwiftTerm and SwiftUIX. Keep dependency updates isolated and avoid unrelated edits.
- `NewTerm.xcodeproj/` defines the shared iOS, macOS/Catalyst, Common, and Theos schemes. Packaging uses the root `Makefile` and `control` file.

## Build, Test, and Development Commands

- `open NewTerm.xcodeproj` opens the project and resolves package dependencies through Xcode.
- `xcodebuild -project NewTerm.xcodeproj -scheme "NewTerm (iOS)" -sdk iphonesimulator build` performs a Simulator build.
- `gmake package THEOS=~/theos` reproduces CI packaging and creates a Debian package under `packages/`.
- `make do` builds and installs on a configured jailbroken device; Theos must be installed.
- `make clean` removes Xcode/Theos output and generated packages.

## Coding Style & Naming Conventions

Follow the surrounding Swift style: tabs for indentation, braces on the declaration line, and `// MARK:` sections for larger types. Use `UpperCamelCase` for types and `lowerCamelCase` for properties and methods. Match file patterns such as `TerminalController+ITermExtensions.swift`, `SettingsPerformanceView.swift`, and `TerminalSceneDelegate.swift`. Guard platform-specific code with availability checks or conditional compilation. No repository-wide formatter or linter is configured; review Xcode warnings and avoid formatting-only churn.

## Testing Guidelines

There is no first-party XCTest target. Build the affected scheme and manually verify behavior on the “My Mac” destination or an iOS Simulator. For terminal changes, exercise process launch, text input, resizing, tabs, and split panes. For UI changes, check phone and iPad layouts plus light/dark appearance. Add focused XCTest coverage when introducing independently testable logic.

## Commit & Pull Request Guidelines

Keep commits concise and imperative. History uses optional scopes such as `[app] Fix keyboard repeat` or `[common] Improve subprocess errors`; prefer these over vague messages like “bug fixes.” Pull requests should explain the user-visible effect, list tested destinations and commands, link issues, and include screenshots or recordings for UI work. Note rootful, rootless, or roothide packaging implications when applicable.

## Security & Generated Files

Do not commit signing credentials, provisioning profiles, device identifiers, `.theos/`, `packages/`, or `xcuserdata/`. Preserve the iOS 14 deployment target and existing entitlement/signing behavior unless explicitly changing them.
