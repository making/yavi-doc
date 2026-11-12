# Bump the documentation to YAVI 0.17.3

Difficulty: Low

Dependencies: none for the edits. The build verification at the end requires
`002-make-doc-build-reproducible.md`.

## Goal

The reference documentation still declares and describes YAVI 0.9.1. Make the
version metadata and the getting-started dependency versions match YAVI 0.17.3
(`../yavi` tag `0.17.3`).

## Changes

- `pom.xml`: change `<version>0.9.1</version>` to `0.17.3`. The doc's
  `yavi.version` property and the `project-version` AsciiDoc attribute are derived
  from it, so no other attribute needs editing.
- `src/main/asciidoc/getting-started.adoc`: update every hard-coded dependency
  version:
  - `am.ik.yavi:yavi` 0.9.1 -> 0.17.3
  - `junit-jupiter-api` 5.8.1 -> 5.14.4 (the version used by YAVI 0.17.3)
  - `assertj-core` 3.21.0 -> 3.27.7 (the version used by YAVI 0.17.3)
- Confirm the prerequisite text is still true. YAVI 0.17.3 still targets Java 8
  (`../yavi/pom.xml` `java.version` = 1.8); the build of this doc uses Java 11.
  Do not raise the documented runtime requirement to 11 unless YAVI itself does.
- Search for other stale version references:
  `grep -rn "0\\.9\\.1" src pom.xml`.

## Out of scope

- No prose rewrite here. The "Always Valid model" tutorial for getting-started is
  handled in `004-rework-arguments-validator-docs.md`.
- The build currently fails before rendering; `002-make-doc-build-reproducible.md`
  must be done before the verification below can pass.

## Verification

- `grep -rn "0\\.9\\.1" src pom.xml` returns nothing.
- After 002, `./mvnw clean package` produces `docs/index.html` containing
  `0.17.3` in the revision number.
