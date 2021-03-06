# Custom Modules for Typedoc

[![Build Status](https://travis-ci.org/convergencelabs/typedoc-plugin-custom-modules.svg?branch=master)](https://travis-ci.org/convergencelabs/typedoc-plugin-custom-modules)

By default, Typedoc (with the `mode="modules"` option) will create a [Module](https://www.typescriptlang.org/docs/handbook/modules.html) ([née "External Module"](https://github.com/TypeStrong/typedoc/issues/109)) for every file with any exported ES6 constructs. This can result in an unmanageable amount of globals, poor discoverability, and difficult navigation. This plugin enables the grouping of these modules by a particular name.

This plugin supports two additional comment tags:

- `@moduledefinition ModuleName` can be placed at the top of a file that best represents a "module" for your codebase. This will create a `ModuleName` module in the top-level project hierarchy.
- `@module ModuleName` can be added to the comment of any other valid exported Typescript declaration (e.g. a class, interface, function, enum etc). These declarations will be moved to any modules specified with `@moduledefinition`. Any orphaned modules (e.g. a file that exports a `@module`-tagged class and nothing else) are deleted.

Additionally, **all** exported TS constructs not explicitly tagged with `@module` are automatically unwrapped from the default "module" (which is just the file in which it is defined) and placed directly beneath the project. This should be identical to using Typedoc with `mode="file"`.

See the [`test/multiple-ancestor-modules`](/test/multiple-ancestor-modules) directory for a typical use case for this plugin.

Requires typedoc 0.16.0 (yet unreleased)! This is currently built on top of [Typedoc PR #1157](https://github.com/TypeStrong/typedoc/pull/1157), which enables the support of export declarations.

Inspired by the popular [typedoc-plugin-external-module-name](https://github.com/christopherthielen/typedoc-plugin-external-module-name), but with a slightly different set of requirements. This plugin leverages some improved TypeDoc comment APIs to support spaces within module names.

## Companion theme

See the [companion theme](https://github.com/convergencelabs/typedoc-theme) for an optional, slightly customized version of the default theme with this plugin in mind.

### Potential future enhancements:

- Support per-file `@module` tags as well. Individual export `@module`s would override any file `@module`s
- Nested modules with a `@parent` tag or something
- Support automatic creation of modules (such that a `@moduledefinition` isn't required for every potential `@module`)
