# Final verification of the 0.17.3 documentation update

Difficulty: Medium

Dependencies: `001`, `002`, `003`, `004`, `005`, `006`, `007`, `008`, `009`
and `010` must all be complete. Run this todo last.

## Checklist

- `./mvnw clean package` succeeds without network credentials and produces
  `docs/index.html`.
- The rendered book contains a `0.17.3` revision number and every new section
  from `006`-`009` in the table of contents.
- All AsciiDoc cross-references resolve: search for `<<` and verify the target
  ids exist (the build reports missing references as warnings; treat them as
  errors for this task).
- No stale API remains anywhere in `src/main/asciidoc`:
  - `BiValidator`, `BiValidatorFactory`
  - `Argument1Validator` (missing `s`)
  - `andThen(` used with a function argument instead of `map`
  - `ValidatorSubset`, `Either`, `ValidatorBuilder#clone`, `isLocalDate`,
    `isIsoLocalDate`, `negaitveOrZero`
  - `0.9.1` in version snippets
- Compile every Java snippet against `am.ik.yavi:yavi:0.17.3` and every Kotlin
  snippet with the Kotlin version from `../yavi/pom.xml`. A scratch project
  outside this repository is enough; no test project needs to be committed.
- Run representative validations for the examples whose output messages are
  asserted in the docs, and fix any message mismatch.
- Review `git diff` for accidental deletions, duplicated sections or broken
  table formatting.

## Completion

When all items pass, remove the completed `.todo` files and record each one in
`.todo/.history.md` (file path, date, status, the commit that removed the file,
and the model that did the work). Use a separate commit for the removal and a
separate commit for the history row, as required by the global instructions.
