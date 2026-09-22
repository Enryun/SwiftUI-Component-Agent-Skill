# Compatibility and verification

## Inspect the host first

Read Package.swift and relevant target/project settings before choosing APIs. Record supported platforms, deployment targets, Swift tools version, language mode, and existing preview/test conventions. These are separate constraints; do not infer them from the installed Xcode version.

Preserve those constraints unless the user requests a migration. Do not raise minimum versions, enable new language features, or add a custom helper just to copy an example.

## Availability

- Prefer an API supported by the target when it meets the requirement.
- For a newer API, choose a supported fallback or explicitly availability-gate the affected public API according to the package's contract.
- Keep each feature's availability decision localized. A single global branch is not required for unrelated features.
- Runtime `#available` does not make unsupported syntax, macros, or missing platform modules compile. Check compiler/toolchain support and use conditional compilation for platform-specific imports where needed.
- `.apply` is not a standard SwiftUI modifier; use it only if the host defines it. Otherwise use ordinary `@ViewBuilder` branches or a focused helper.
- Use `#Preview` only when supported by the host toolchain and target; `PreviewProvider` is a useful compatible alternative. Do not add NavigationStack solely to display a sample.

## Verification proportional to the change

1. Build the affected library target for its supported platform using the host's existing commands. A macOS `swift build` does not verify an iOS-only target.
2. Compile sample usage as a separate consumer module when changing public API, so access-control mistakes are visible.
3. Run existing relevant tests; add focused behavioral coverage for meaningful changes such as state ownership, validation, or event timing.
4. For visual or interaction changes, check the affected sample: default and custom appearance, input behavior, relevant accessibility, and inherited modifier scope. Exercise fallback paths on an available supported runtime.
5. Report exactly what ran and what could not be checked. Compilation does not prove rendering or interaction; do not claim preview/simulator success without running it.

Sources: [Swift package manifest reference](https://docs.swift.org/package-manager/PackageDescription/PackageDescription.html), [Swift language mode guidance](https://www.swift.org/migration/documentation/swift-6-concurrency-migration-guide/enabledataracesafety/).
