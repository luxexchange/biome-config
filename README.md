# @luxfi/biome-config

Shared [Biome](https://biomejs.dev) linter and formatter configuration for the
Lux Exchange workspace.

## Overview

This package provides a single source-of-truth Biome configuration consumed by
every package and app in the monorepo. The authored config lives in
`base.jsonc`; a compiled, marker-resolved `compiled/base.json` is published for
direct consumption.

### Generic marker system

`base.jsonc` uses `__INCLUDE_GLOBAL_VALUES__` markers in `overrides` to merge
values from the main linter config. `scripts/generate.js` compiles the authored
config into `compiled/base.json` by:

1. Extracting all rule values from the main `linter.rules` section.
2. Resolving `__INCLUDE_GLOBAL_VALUES__` markers in override sections.
3. Merging the global values into the override at the same rule path.
4. Stripping the markers from the compiled output.

Only top-level keys are merged (first-level). Setting a key to `"off"` removes
that specific global entry from an override; array options are deduplicated by
JSON serialization.

## Usage

Extend the compiled config from your `biome.json`:

```json
{
  "extends": ["@luxfi/biome-config/compiled/base.json"]
}
```

## License

BSD-3-Clause © 2020-2026 Lux Industries, Inc. See [LICENSE](./LICENSE).
