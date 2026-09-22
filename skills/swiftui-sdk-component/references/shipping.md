# Shipping

Foundation **#5** — stable, discoverable public API.

## Visibility and compatibility

- Expose only the component and supporting types/members consumers need. Public types alone do not make their initializers and members public.
- Keep helpers and implementation details internal or private.
- Preserve existing public names, signatures, defaults, and behavior during unrelated improvements. A legacy typo is not permission for a breaking rename.
- For an authorized API migration, describe the compatibility impact and follow the package's deprecation/migration policy.
- Expose platform-specific types only when the component's contract intentionally bridges that platform.

## Package integration

Inspect the existing manifest and target layout. Swift package products can contain multiple targets; do not impose one module per product. Respect custom source paths, exclusions, resources, and explicit source lists when adding files.

Prefer existing dependencies and system frameworks. Add a dependency only when justified by the task. Isolate platform side effects, and document permissions or resource setup only for APIs that actually need them.

## Releases

Component implementation does not authorize a release. Do not bump versions, edit release markers, create tags, publish packages, or rebuild/distribute binaries unless that work is requested or part of an explicitly authorized release workflow.

When releasing, follow the repository's versioning and binary distribution conventions. Do not assume a runtime constant such as CommonSwiftUI.version is the package release version. Report breaking changes and required migration steps before release.

## Consumer checks

Compile representative use from outside the library module; verify required supporting members are accessible. Confirm new files and resources belong to the intended target. Use [platform-versioning.md](platform-versioning.md) for platform and runtime verification.

Avoid app-specific asset dependencies, debug output, and fixed user-facing copy that prevents host localization.

Source: [Swift package products and targets](https://docs.swift.org/latest/documentation/packagemanagerdocs/introducingpackages/).
