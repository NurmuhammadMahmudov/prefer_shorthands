# ndev_prefer_shorthands

A **Flutter 3.44+**–compatible fork of [prefer_shorthands](https://pub.dev/packages/prefer_shorthands). It is an **analyzer plugin** that suggests Dart **dot shorthand** (for example `.min` instead of `MainAxisSize.min` when the type is already known, like `mainAxisSize: .min`).

---

## Who changed this package, and why?

**Original:** [prefer_shorthands 0.4.7](https://pub.dev/packages/prefer_shorthands) by the original author ([source](https://github.com/huanghui1998hhh/prefer_shorthands)).

**Problem on Flutter 3.44:** pub.dev `0.4.7` needs `analyzer_plugin` **0.13.x**. Flutter 3.44’s analysis server ships **0.14.x**. The plugin does not load — this is a toolchain mismatch, not your app bug.

**This fork (0.4.8):** Updated by **[NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov)** for Flutter 3.44+ ([GitHub repo](https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands)).

| | pub.dev 0.4.7 | This fork 0.4.8 |
|---|---------------|-----------------|
| Flutter 3.44+ | No | **Yes** |
| Source | pub.dev | GitHub tag `v0.4.8` |

- GitHub: [NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov)
- LinkedIn: [nurmuhammad-mahmudov](https://www.linkedin.com/in/nurmuhammad-mahmudov-b2813b355/)

---

## Can I add it with only a GitHub URL?

**Short answer for Flutter 3.44 users: no — not yet.**

Analyzer plugins are enabled in **`analysis_options.yaml`**, not in `pubspec.yaml`. The analysis server supports:

| How you point to the plugin | Flutter 3.44 (Dart 3.12) | Dart 3.13+ |
|-----------------------------|--------------------------|------------|
| `git:` + GitHub URL in `analysis_options.yaml` | **Not supported** | **Supported** |
| `path:` to a folder on your machine | **Supported** (use this) | Supported |

So on **Flutter 3.44** you clone this repo **once** into your project (see below), then reference that folder with `path:`. You cannot paste a GitHub URL as `path:` — `path` must be a real directory on disk.

You also **must not** add this package under `pubspec.yaml` → `dependencies` / `dev_dependencies`; that does not load analyzer plugins and can break other tools.

---

## Add to your project (Flutter 3.44 — recommended)

Do this at the **root** of your Flutter app (where `pubspec.yaml` lives).

### 1. Clone into `packages/`

```bash
git clone https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands.git packages/prefer_shorthands
cd packages/prefer_shorthands && git checkout v0.4.8 && cd ../..
```

Your tree:

```text
my_app/
├── lib/
├── pubspec.yaml
├── analysis_options.yaml          ← edit this
└── packages/
    └── prefer_shorthands/         ← this repo (must contain pubspec.yaml)
```

Commit `packages/prefer_shorthands` (or use a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) at the same path if your team prefers).

### 2. Enable in root `analysis_options.yaml`

```yaml
plugins:
  prefer_shorthands:
    path: packages/prefer_shorthands
    diagnostics:
      prefer_shorthands: true

prefer_shorthands:
  convert_implicit_declaration: false   # optional
```

- `diagnostics.prefer_shorthands: true` — required; plugin lints are off by default.
- Keep any existing `include:` / `linter:` blocks; only add the sections above.

### 3. Restart analysis

Command Palette → **Dart: Restart Analysis Server** (or restart the IDE).

### 4. Check it works

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

```bash
flutter analyze
```

---

## GitHub URL only (Dart 3.13+ / future Flutter)

If your SDK supports `git:` for analyzer plugins ([dart-lang/sdk#61794](https://github.com/dart-lang/sdk/issues/61794) — fixed in Dart 3.13 Beta 1+), you can skip cloning and use:

```yaml
plugins:
  prefer_shorthands:
    git:
      url: https://github.com/NurmuhammadMahmudov/ndev_prefer_shorthands.git
      ref: v0.4.8
    diagnostics:
      prefer_shorthands: true
```

Then restart the analysis server. **On Flutter 3.44 this often does nothing** — use the `packages/` + `path:` steps above instead.

---

## Older Flutter (< 3.44)

Use the original from pub.dev in root `analysis_options.yaml`:

```yaml
plugins:
  prefer_shorthands: ^0.4.7
  # plus diagnostics: prefer_shorthands: true under the plugin key if needed
```

---

## License

MIT — original author **hhh** (2025). Fork: [NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov). See [LICENSE](LICENSE).
