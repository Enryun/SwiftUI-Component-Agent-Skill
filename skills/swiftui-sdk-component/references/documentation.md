# Documentation and comments

Document the public contract, not obvious implementation steps.

## Public API

Give the main public type a concise summary and one self-contained usage example. Describe non-obvious requirements, callback timing, state ownership, modifier scope, and defaults where relevant. Scale the detail to the API; do not repeat the signature as prose.

Examples must include required supporting types and state. Do not reference undefined errors, unavailable modifiers, or API from a different version. The bundled [component template](../templates/swift/Component.swift.template) includes a minimal consumer example.

For a public environment modifier, name the affected component family, explain descendant scope and defaults, and state how local overrides work. A short comment is sufficient for a simple contract.

## Implementation comments

- Do not add comments that merely narrate code: “Update the value,” “Create the button,” or “Initialize properties.”
- Prefer clear names and small functions over explanatory prose for routine logic.
- Keep comments that explain a non-obvious reason, invariant, compatibility workaround, or tradeoff that the code cannot express.
- Do not generate file banners, section markers, or parameter descriptions mechanically. Follow useful host conventions without adding noise.
- When changing behavior, update or remove stale comments. Do not perform unrelated comment cleanup.

Instructional BAD/GOOD annotations in this skill are teaching aids, not comments to copy into generated source.

## Samples and README

Use the host's existing sample or preview arrangement. Demonstrate default usage and the customization actually supported. Do not create a new sample app solely to satisfy a folder convention. Avoid duplicating the entire reference documentation into code comments or the package README.
