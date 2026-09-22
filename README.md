# SwiftUI Component Agent Skill

Help your coding agent build SwiftUI components that are **easy to reuse, straightforward to customize, and safe to evolve**.

## Six principles for reusable components

### 1. Separate behavior from appearance

Keep the component's behavior independent of app branding. Use styles, modifiers, or grouped configuration for the appearance callers need to change.

**Benefit:** reuse the same component across different designs without copying its logic.

### 2. Give each value one owner

Accept values for read-only inputs, bindings for parent-owned values the component edits, and focused callbacks for meaningful actions. Keep transient internal state private.

**Benefit:** predictable updates, fewer synchronization bugs, and fewer unnecessary callbacks.

### 3. Keep the API focused

Make required inputs explicit. Add customization for real needs, with clear names and scope. Use environment settings when options should flow to descendant components.

**Benefit:** call sites are easier to understand, and new options can be added without growing an initializer full of unrelated flags.

### 4. Build through composition

Reuse SwiftUI controls and style protocols. Choose a View, ButtonStyle, or ViewModifier to match the responsibility. Let callers supply custom content through focused builder slots when needed.

**Benefit:** new shapes, styles, or content can fit the same component while its reusable behavior stays in one place.

### 5. Keep public contracts stable

Expose what callers need and keep implementation details private. Preserve existing usage when improving the internals.

**Benefit:** components can evolve without forcing changes throughout the apps that use them.

### 6. Make the default useful

Basic usage should work with required data and actions, without extra styling setup. Customization should be explicit and have a visible or behavioral effect.

**Benefit:** components are easy to adopt and remain adaptable as requirements grow.

Apply these principles with the simplest suitable implementation. A Config, environment key, or extra helper earns its place by solving a real need.

> Can a realistic new appearance or content variation be supported without duplicating the component or adding unrelated flags and callbacks?

Derived from experience building [CommonSwiftUI](https://github.com/Enryun/Common_SwiftUI). Explore the [detailed principles](skills/swiftui-sdk-component/references/six-foundations.md), [composition guidance](skills/swiftui-sdk-component/references/composition.md), and [review checklist](COMPONENT-CHECKS.md).

## Use it with your agent

After installation, ask for the outcome you need. You do not need to prescribe Config types, modifiers, or extra layers.

**Create a component**

> Use swiftui-sdk-component to build a reusable loading button for this package. Follow our existing API and platform conventions, keep the caller's action and label customizable, and demonstrate default and customized usage.

**Extend a component**

> Use swiftui-sdk-component to add a custom accessory to this field. Preserve existing call sites and choose the smallest API change that supports it.

**Review a component**

> Review this component with swiftui-sdk-component. Focus on state ownership, unnecessary callbacks, customization boundaries, and whether the public API will be easy to evolve. Report findings without editing files.

The agent inspects the host, explains material design decisions, implements the requested scope, and verifies the result. It should report checks it could not perform. An implementation request does not require another routine approval checkpoint; if you want a proposal first, say so.

## Installation

From a checkout of this repository, copy the skill folder into your tool's skills directory. It contains all references and Swift templates; the original repository is not needed after installation.

### Cursor: personal

```bash
mkdir -p ~/.cursor/skills
cp -R skills/swiftui-sdk-component ~/.cursor/skills/
```

### Cursor: project-local

```bash
mkdir -p .cursor/skills
cp -R skills/swiftui-sdk-component .cursor/skills/
```

For other tools supporting the Agent Skills format, use their configured skills directory.

An optional [Cursor rule](templates/component.mdc) can be copied into `.cursor/rules/`. It is opt-in, with `alwaysApply: false`.

## Scope and resources

This skill focuses on reusable SwiftUI components. For app feature layers and dependency composition, see [SwiftUI Architecture Agent Skill](https://github.com/Enryun/SwiftUI-Architecture-Agent-Skill). For substantial concurrency work, see [Swift Concurrency Agent Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill).

| Resource | Purpose |
|----------|---------|
| [SKILL.md](skills/swiftui-sdk-component/SKILL.md) | Agent entry point and core guidance |
| [Reference index](skills/swiftui-sdk-component/references/_index.md) | Detailed guidance for specific decisions |
| [Component workflow](skills/swiftui-sdk-component/references/scaffold-workflow.md) | Design, implementation, and verification |
| [Bundled templates](skills/swiftui-sdk-component/templates/README.md) | Working starting points to adapt, not mandatory structures |
| [Contributing](CONTRIBUTING.md) | Scope and contribution guidance |

MIT licensed. See [LICENSE](LICENSE).
