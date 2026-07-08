# Changelog

## 0.5.0

- **Flutter 3.44.2 fix:** pin `analysis_server_plugin` to `<0.3.15` (analyzer 12.x) to avoid [SDK #63538](https://github.com/dart-lang/sdk/issues/63538) where plugin host 0.3.15+ hangs or silently skips rules on Dart 3.12.2
- Revert AST visitor code to analyzer 12 API (compatible with `analysis_server_plugin` 0.3.14)
- Add `example/analysis_options.yaml` with required `diagnostics.prefer_shorthands: true`
- Fix generic argument false positives (`List.filled`, etc.)

## 0.4.9

- Fix false positives in generic argument positions (e.g. `List.filled(5, EnumA.a)`) by restoring the `hasExplicitTypeContext` check for `TypeParameterType` parameters
- Add regression tests from the original `prefer_shorthands` package

## 0.4.8

- Flutter 3.44+ / Dart 3.12+ analysis server bilan moslik
- `analyzer_plugin` 0.14.x va analyzer 13 AST API ga moslashtirildi
- [pub.dev prefer_shorthands](https://pub.dev/packages/prefer_shorthands) **0.4.7** dan fork — [NurmuhammadMahmudov](https://github.com/NurmuhammadMahmudov)
