# Component workflow

Use this workflow for creating or substantially extending a public component. Scale the detail to the change.

## 1. Inspect and design

Read the host's instructions, target settings, nearby components, existing public API, and sample/test conventions. Review all six foundations internally; communicate the decisions that matter.

For a new component, outline:

- Responsibility and intended consumers.
- Required values, editable bindings, local state, and any justified callbacks.
- Public initializer and customization API; apply preferred patterns when their conditions hold.
- Modifier names, affected component family, per-instance versus inherited scope, defaults, and override behavior.
- Files needed, including a consumer example in the host's existing sample/preview arrangement.
- Platform/toolchain constraints and relevant interaction or accessibility cases.

Do not add Config, environment keys, helper types, or sample apps solely to fill this outline.

### Authorization

A request to implement or fix already authorizes ordinary work within that scope. Briefly state the approach and continue; do not require a separate approval for every component or file.

Stop after the proposal only when the user requested proposal-only work. Ask a focused question when a material unresolved choice changes the public contract or would expand scope, such as an unrequested breaking change or dependency. Honor prior decisions and approval; do not ask again.

For a small compatible edit, a short explanation is enough. For an authorized breaking change, explain compatibility and migration implications before implementation.

## 2. Implement selected mechanisms

Work in dependency order and follow the host layout.

### Data flow

- Use values for read-only inputs and Binding for edits to parent-owned state.
- Keep component-owned transient state private; do not mirror bindings or use constant bindings as editable fallbacks.
- Justify each callback with a concrete consumer need; specify event timing and payload.
- Avoid redundant state notifications, callback chains, and collections of workflow closures.
- Apply validation-specific guidance only to components that validate input.

### Customization and composition

- Prefer suitable system controls and standard styling.
- Prefer nested Config for coherent groups of component-specific appearance settings, with usable defaults and appropriate access control.
- Use environment keys only for intentionally inherited settings; scope public modifier names to their component family and implement a consumer.
- Extract subviews around distinct responsibilities, not numeric size thresholds.
- Choose View, standard style, or ViewModifier to match the reusable responsibility. Use existing protocols and focused content slots for demonstrated variation; check a realistic extension without adding speculative API.
- Isolate platform side effects in focused helpers when needed. Add permission keys only for APIs that require them.

### Compatibility and docs

- Preserve deployment targets, language settings, and compatible public API.
- Localize availability decisions and reuse only helpers that exist in the host.
- Document public contracts with a self-contained usage example.
- Keep comments for non-obvious reasons or constraints; omit comments that narrate obvious code.
- Follow [platform-versioning.md](platform-versioning.md), [documentation.md](documentation.md), and [shipping.md](shipping.md).

### Sample

Use existing sample or preview infrastructure. Show basic usage with required inputs and no styling setup, supported customization, and relevant edge cases. For an inherited modifier, demonstrate ancestor application and a local override. Every demonstrated option must have an observable effect.

The bundled [templates](../templates/README.md) are optional starting points. Remove unused sections and replace every placeholder.

## 3. Review and verify

- All six foundations hold, with meaningful departures from preferred patterns justified.
- Required inputs and ownership are clear; callback scope is focused.
- Every public option is consumed; inherited scope and local overrides match documentation.
- Presentation choices preserve underlying state.
- Existing APIs and package settings remain compatible unless migration was authorized.
- Documentation examples are complete and comments add useful information.
- No debug noise, unused state, app-specific assets, or unexplained dependencies.
- Build the affected target and compile a separate consumer for public API changes.
- Run relevant behavioral tests and inspect affected interactions/rendering where available.

Report files changed, resulting API/behavior, checks actually run, and verification gaps. Do not report skipped previews or simulator checks as passing. Do not create releases, version bumps, tags, or publications unless requested.

## Optional proposal outline

For tasks that benefit from an explicit proposal:

1. Responsibility and consumer example.
2. Data-flow ownership.
3. Public API and customization scope.
4. Relevant foundation decisions and tradeoffs.
5. Files and host integration.
6. Compatibility, sample coverage, and verification.

End with a question only if a decision or authorization is actually missing.
