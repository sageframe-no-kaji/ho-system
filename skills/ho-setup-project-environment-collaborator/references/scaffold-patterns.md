# Scaffold Patterns

Concrete file content for the boilerplate this skill creates. These are not the templates the practitioner has in their practitioner templates directory—those handle pyproject.toml, pre-commit, gitignore, env.example, project instructions. These are the additional files the skill writes when scaffolding (smoke test, package init, README stub).

## Python: `src/[package]/__init__.py`

```python
"""[package-name]: [one-line project description]."""

__version__ = "0.1.0"
```

That's it. The `__init__.py` exists so the package is importable; the version string is conventional. The docstring says what the package is.

## Python: `tests/test_smoke.py`

```python
"""Smoke test that proves the verification stack runs.

Replace with real tests as soon as there's behavior to specify.
"""


def test_package_imports() -> None:
    """The package is importable."""
    import [package_name]  # noqa: F401  # smoke test


def test_truth() -> None:
    """Truth is truth."""
    assert True
```

The smoke test exists so:

1. `pytest` has something to run; the practitioner can prove the verification stack works before any real code.
2. The coverage floor (90%) doesn't immediately fail on an empty test suite. Once real code is added, real tests replace this.
3. The `noqa: F401` is there because importing without using is what we're testing.

## README.md (Python project)

```markdown
# [project-name]

[one-line description]

## Setup

```bash
uv venv .venv
uv sync --all-groups
pre-commit install
```

## Verify

```bash
ruff check
mypy
pytest
```

## Run

[how to run the project — fill in once the entry point exists]
```

The README is short by design. The practitioner expands it as the project develops.

## README.md (web project)

```markdown
# [project-name]

[one-line description]

## Setup

```bash
npm install
```

## Develop

```bash
npx [eleventy or whatever the dev command is]
```

## Build

```bash
npx [build command]
```

## Deploy

[deploy command, e.g., `npx wrangler pages deploy public`]
```

## Dockerfile (when project type is service with Docker deployment)

The Dockerfile pattern depends on the practitioner's docker-deploy skill. If `sageframe-docker-deploy` is available, invoke it to generate the Dockerfile and docker-compose.yml as part of the scaffold. If it isn't, generate a minimal pattern:

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.11-slim

WORKDIR /app

# System dependencies (add as needed)
RUN apt-get update && apt-get install -y --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Install uv
RUN pip install --no-cache-dir uv

# Copy project
COPY pyproject.toml uv.lock ./
COPY src/ ./src/

RUN uv sync --frozen --no-dev

EXPOSE 8000

CMD ["uv", "run", "python", "-m", "[package_name].main"]
```

This is a starting point; the practitioner adapts to their specific deployment.

## tailwind.config.js (when project includes Tailwind)

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/[package]/templates/**/*.html",
    "./src/[package]/static/js/**/*.js",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

## package.json (when project includes web build pipeline)

For Tailwind builds in an app frontend:

```json
{
  "name": "[project-name]",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "build:css": "tailwindcss -i src/[package]/static/css/input.css -o src/[package]/static/css/output.css",
    "watch:css": "tailwindcss -i src/[package]/static/css/input.css -o src/[package]/static/css/output.css --watch"
  },
  "devDependencies": {
    "tailwindcss": "^4.0.0",
    "@tailwindcss/cli": "^4.0.0"
  }
}
```

For Eleventy static sites:

```json
{
  "name": "[project-name]",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "build": "eleventy",
    "serve": "eleventy --serve",
    "deploy": "npm run build && npx wrangler pages deploy public"
  },
  "devDependencies": {
    "@11ty/eleventy": "^3.0.0"
  }
}
```

## .eleventy.js (Eleventy static site)

```javascript
export default function (eleventyConfig) {
  eleventyConfig.addPassthroughCopy("src/static");

  return {
    dir: {
      input: "src",
      includes: "_includes",
      data: "_data",
      output: "public",
    },
    pathPrefix: "/",
  };
}
```

## src/index.njk (Eleventy index page)

```njk
---
layout: base.njk
title: [project-name]
---

# [project-name]

[one-line description]
```

Plus a minimal `src/_includes/base.njk`:

```njk
[!doctype html]
[html lang="en"]
[head]
  [meta charset="utf-8"]
  [meta name="viewport" content="width=device-width, initial-scale=1"]
  [title]{{ title }}[/title]
[/head]
[body]
  {{ content | safe }}
[/body]
[/html]
```

The practitioner customizes from here.

## Swift: `Sources/[PackageName]Core/[PackageName]Core.swift`

```swift
// [PackageName]Core: [one-line project description].

public enum [PackageName]Core {
    public static let version = "0.1.0"
}
```

The library target needs at least one file; the enum gives the namespace something to live under and a conventional version constant.

## Swift: `Tests/[PackageName]CoreTests/[PackageName]CoreTests.swift`

```swift
import Testing
@testable import [PackageName]Core

@Suite("Smoke")
struct SmokeTests {
    @Test("Module is importable and version is set")
    func versionIsSet() {
        #expect(![PackageName]Core.version.isEmpty)
    }

    @Test("Truth is truth")
    func truth() {
        #expect(true)
    }
}
```

Uses swift-testing (`@Test`, `#expect`). The smoke suite exists so:

1. `swift test` has something to run; the practitioner can prove the verification stack works before any real code.
2. The coverage measurement has a baseline; once real code is added, real tests replace this.
3. swift-testing is the default for new test code per the practitioner's Swift language module (`languages-swift.md`); XCTest stays available alongside for XCUITest UI testing.

## README.md (Swift project)

````markdown
# [project-name]

[one-line description]

## Setup

Requires the Swift 6.0+ toolchain (Xcode 16+ on macOS, or `swiftly` on Linux).

```bash
swift build
```

## Verify

```bash
swift-format lint --recursive --strict Sources Tests
swiftlint lint --strict
swift build
swift test
```

## Run

[how to run the project — `swift run [executable-name]` once an executable target exists]
````

## Swift app target: `Sources/[PackageName]App/App.swift` (when the project has a SwiftUI app)

```swift
import SwiftUI

@main
struct [PackageName]App: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

struct ContentView: View {
    var body: some View {
        Text("[project-name]")
            .frame(minWidth: 480, minHeight: 320)
    }
}
```

For non-trivial apps an `.xcodeproj` typically lives at `xcode/[PackageName].xcodeproj/` and references the Swift package via Local Package Reference — Xcode owns asset catalogs, Info.plist beyond what SwiftPM handles, and the notarization workflow.

## Swift CLI target: `Sources/[PackageName]CLI/main.swift` (when the project has a CLI)

```swift
import ArgumentParser

@main
struct [PackageNameLower]: AsyncParsableCommand {
    static let configuration = CommandConfiguration(
        commandName: "[package-cli-name]",
        abstract: "[one-line CLI description]",
        subcommands: [],
        defaultSubcommand: nil
    )

    func run() async throws {
        print("[project-name] CLI")
    }
}
```

Subcommands compose as nested `ParsableCommand` types referenced in the `subcommands:` array.

## When Scaffolding Crosses Languages

For app frontends (Python + web), generate both the Python source layout AND the package.json/tailwind.config.js. The project instructions load both language modules. The verification stack covers the Python side; the web side runs through whatever toolchain the practitioner uses.

For Python services with optional web admin UIs: scaffold Python first; offer web later when it becomes relevant. Don't pre-scaffold infrastructure the project doesn't yet need.
