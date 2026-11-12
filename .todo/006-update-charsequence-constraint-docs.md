# Update the CharSequence constraint documentation

Difficulty: Medium

Dependencies: none. This todo edits the String section of
`built-in-constraints.adoc`; `007` and `008` edit other sections of the same
file, so coordinate concurrent edits.

## Goal

Add the string constraints introduced after 0.9.1 to
`src/main/asciidoc/built-in-constraints.adoc`, following the existing format
(example, expected message, message-key table with argument descriptions).
Use the 0.17.3 source in `../yavi` as the source of truth.

## Sections to add under `=== String`

- `startsWith(CharSequence)` (0.10.0). Message key
  `charSequence.startsWith`; `{1}` is the prefix.
- `endsWith(CharSequence)` (0.10.0). Message key `charSequence.endsWith`;
  `{1}` is the suffix.
- `uuid()` (0.10.0). Checks the 8-4-4-4-12 hexadecimal format; an empty value
  is valid; version/variant are not checked. Message key
  `charSequence.uuid`.
- `isoLocalDate()` (0.12.1) and `localDate(String pattern)` (0.12.1). Both use
  `ResolverStyle.STRICT`; the pattern is `uuuu-MM-dd` for `isoLocalDate()`.
  Message key `charSequence.localdate`; `{1}` is the pattern.
- `pattern(Pattern)` and `pattern(Supplier<Pattern>)` (0.11.1). Explain that
  the supplier is invoked for every validation and is intended for
  dynamically built regexes. Same message key as `pattern(String)`.
- `url(Function<UrlConstraintBuilder, List<ConstraintPredicate>>)` (0.17.1)
  with `.protocol(...)`, `.host(...)`, `.port(...)` and `.build()`.
  Show the Bean Validation `@URL(protocol = "https", host = "example.com",
  port = 8443)` equivalent. Document the message keys
  `charSequence.url.protocol`, `charSequence.url.host` and
  `charSequence.url.port` in addition to `charSequence.url`.
- Update `url()` itself: since 0.17.0 the value is parsed as a `URI` instead of
  the deprecated `URL(String)` constructor, so leading/trailing characters are
  no longer silently trimmed, unencoded characters are rejected, and an empty
  authority is rejected. Note this behavior change.
- `variant(Function<VariantOptions.Builder, VariantOptions.Builder>)`
  (long-standing, but missing from the docs) near the existing
  emoji/code-points material. It removes selected variation
  selectors before size measurement, e.g.
  `c.variant(opts -> opts.fvs(MongolianFreeVariationSelector.IGNORE)).fixedSize(1)`.
  Cross-reference the existing "Advanced character length check" material
  instead of duplicating it.
- Mention `_charSequence` as the explicit-inference builder shortcut if the
  String section does not already describe shortcuts.
- Fix the `email()` example comment: it currently says
  `// Invalid ("email" must not be a valid email address)`; the actual message
  is `"email" must be a valid email address`.

## Semantics to state once per section

- A `null` value is valid unless `notNull()` is added.
- `uuid()` treats an empty value as valid; `url()` treats an empty value as
  valid.
- Do not document the removed `isLocalDate` / `isIsoLocalDate` names; the
  current methods are `isoLocalDate()` and `localDate(pattern)`.

## Verification

- Each example's reported message matches the 0.17.3 message key table in
  `../yavi/src/main/java/am/ik/yavi/core/ViolationMessage.java`.
- Compile the snippets against `am.ik.yavi:yavi:0.17.3` in a scratch project.
- Run at least one value per new constraint through a real validator and
  compare the message.
