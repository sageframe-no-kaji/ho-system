# Languages: Swift

Placeholder. Sutra is the first Swift project on the horizon (macOS writing environment).

This module is imported by project-level CLAUDE.md files when the project uses Swift. It is not loaded globally.

When Sutra's implementation begins, the `ho-environment-setup-collaborator` skill will interrogate for stack specifics (Xcode vs SwiftPM, lint configuration, test harness, target macOS versions, signing/notarization) and fill in this module.

Anticipated universal Ho conventions for Swift:

- SwiftPM (Package.swift) as the package manager when the project structure allows
- Xcode for app shells when SwiftUI/AppKit integration requires it
- `swift-format` for formatting
- `SwiftLint` for linting (strict ruleset)
- `swift test` (XCTest) with coverage measurement via `xcrun llvm-cov`
- Coverage floor enforced (target ≥90% with adjustments for boilerplate)
- Type errors are compile errors

Anticipated Andrew specifics for Sutra:

- macOS 13+ target (TBD)
- TextKit 2 vs ProseMirror choice deferred to an early ho spike
- libgit2 integration for the version engine
- Signing and notarization pipeline (mirrors m4bmaker's macOS distribution path)
