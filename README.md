# ndev_prefer_shorthands

A **Flutter 3.44+**–compatible fork of [prefer_shorthands](https://pub.dev/packages/prefer_shorthands). It is an **analyzer plugin** that suggests Dart **dot shorthand** (for example `.min` instead of `MainAxisSize.min` when the type is already known, like `mainAxisSize: .min`).

---

## Who changed this package, and why?

**Original:** [prefer_shorthands 0.4.7](https://pub.dev/packages/prefer_shorthands) by the original author ([source](https://github.com/huanghui1998hhh/prefer_shorthands)).

**Problem on Flutter 3.44:** pub.dev `0.4.7` needs `analyzer_plugin` **0.13.x**. Flutter 3.44’s analysis server ships **0.14.x**. The plugin does not load — this is a toolchain mismatch, not your app bug.

**This fork (0.5.0+):** Updated by **[NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov)** for Flutter 3.44+ ([GitHub repo](https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands)).

| | pub.dev 0.4.7 | This fork 0.5.0 |
|---|---------------|-----------------|
| Flutter 3.44.2 | No | **Yes** (use `dart analyze` or IDE) |
| Source | pub.dev | GitHub tag `v0.5.0` |

- GitHub: [NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov)
- LinkedIn: [nurmuhammad-mahmudov](https://www.linkedin.com/in/nurmuhammad-mahmudov-b2813b355/)

---

## Setup

### Best practice (use this today)

On **Flutter 3.44** (and until your SDK supports `git:` in `plugins:` — see below), the reliable approach is:

1. **Clone** this repo into your app under `packages/prefer_shorthands`
2. Point the plugin with **`path:`** in root **`analysis_options.yaml`**
3. **Restart** the Dart Analysis Server

Do **not** rely on a GitHub URL inside `analysis_options.yaml` on Flutter 3.44 — the Dart SDK does not load plugins that way yet.

Do **not** add this repo under `pubspec.yaml` → `dependencies` / `dev_dependencies`; that does not enable analyzer plugins.

---

### Step 1 — Clone into `packages/`

From your **Flutter app root** (same folder as `pubspec.yaml`):

```bash
git clone https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands.git packages/prefer_shorthands
cd packages/prefer_shorthands && git checkout v0.5.0 && cd ../..
```

```text
my_app/
├── lib/
├── pubspec.yaml
├── analysis_options.yaml          ← edit in step 2
└── packages/
    └── prefer_shorthands/         ← this repo
```

Commit that folder (or use a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) at the same path for teams).

### Step 2 — Root `analysis_options.yaml`

```yaml
plugins:
  prefer_shorthands:
    path: packages/prefer_shorthands
    diagnostics:
      prefer_shorthands: true

prefer_shorthands:
  convert_implicit_declaration: true   # set true to also suggest for `var x = Enum.a`
```

- Plugin config lives only in the **project/workspace root** `analysis_options.yaml`, not in nested package files.
- **`diagnostics.prefer_shorthands: true` is required** — without it you will see **zero warnings**.
- `convert_implicit_declaration: true` enables suggestions for lines like `var a = A()` (no explicit type).

### Step 3 — Restart analysis

Command Palette → **Dart: Restart Analysis Server** (or restart the IDE).

**Important:** after any change to `plugins:` in `analysis_options.yaml`, you must restart the analysis server.

### Step 4 — Verify

```bash
dart analyze .
```

On Flutter 3.44.2, **`flutter analyze` does not show plugin warnings yet** (known SDK issue). Use `dart analyze` in terminal, or check warnings in the IDE after restart.

You should see `prefer_shorthands` info hints, for example on `example/lib/main.dart`:

```bash
cd example && dart analyze .
# info - lib/main.dart:7:13 - Prefer shorthands ...
```

Widget example (type is known from the parameter):

```dart
import 'package:flutter/material.dart';

Widget example() {
  return Row(
    // Before: mainAxisSize: MainAxisSize.min
    // Suggested: mainAxisSize: .min
    mainAxisSize: MainAxisSize.min,
    children: const [Text('Hello')],
  );
}
```

---

## When will `git:` in `analysis_options.yaml` work?

The Dart team added support for **`git:` dependencies under `plugins:`** in the **Dart SDK** (not in this package). It lands with **Dart 3.13 Beta 1 and later** — see [dart-lang/sdk#61794](https://github.com/dart-lang/sdk/issues/61794).

| Your toolchain | `plugins:` + `git:` URL | `plugins:` + `path:` (clone) |
|----------------|-------------------------|----------------------------|
| Flutter 3.44 (Dart 3.12) | **Does not work** | **Works — use this** |
| Dart 3.13+ (future Flutter that bundles it) | **Should work** | Still works |

**How to know when you can switch to a URL:** run `dart --version`. If it is **3.13.0** or higher *and* your IDE/analysis server uses that SDK, you can try the `git:` block below instead of maintaining a local clone. Until then, keep **clone + `path:`**.

**Why `git:` fails on Flutter 3.44:** the analysis server reads `plugins:` and only implements certain sources on Dart 3.12. A `git:` entry is ignored or never resolved, so you see no diagnostics. Cloning gives the server a real folder — same repo, supported via `path:`.

### Future setup — GitHub URL in `analysis_options.yaml` (Dart 3.13+ only)

When your SDK supports it, you can replace the clone step with:

```yaml
plugins:
  prefer_shorthands:
    git:
      url: https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands.git
      ref: v0.4.8
    diagnostics:
      prefer_shorthands: true
```

Restart the analysis server after changing `plugins:`.

Until you have confirmed Dart 3.13+ in your environment, stay on **best practice: clone → `packages/prefer_shorthands` → `path:`**.

---

## Older Flutter (< 3.44)

Use the original from pub.dev in root `analysis_options.yaml`:

```yaml
plugins:
  prefer_shorthands: ^0.4.7
  # enable diagnostics.prefer_shorthands: true under the plugin entry if needed
```

---

## License

MIT — original author **hhh** (2025). Fork: [NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov). See [LICENSE](LICENSE).
