# Update the validator-combination documentation for 0.17.3

Difficulty: Medium

Dependencies: none. It cross-references
`004-rework-arguments-validator-docs.md` and shares
`combining-validation-results.adoc` with `003-update-core-usage-docs.md`;
coordinate concurrent edits to that file.

## Goal

Update `src/main/asciidoc/combining-validators.adoc` and
`src/main/asciidoc/combining-validation-results.adoc` for YAVI 0.17.3. Keep the
existing structure and examples, and add the APIs that appeared after 0.9.1.

## Changes

- `combining-validators.adoc`
  - `ArgumentsValidators.split(...)` and `combine(...)` support up to 16
    arguments (since 0.10.0). State the limit once; no need to enumerate all
    arities.
  - `apply(...)` consumes `Function1` .. `Function16`.
  - `ValueValidator#andThen(ValueValidator)` (0.11.0) can chain validators;
    `map(Function)` (0.17.0) maps the validated value. `andThen(Function)` is
    deprecated.
  - Fix the typo `Argument1Validator` -> `Arguments1Validator`.
  - Document `ArgumentsValidators.traverseN(...)` / `sequenceN(...)` briefly
    with one example, and cross-reference
    `004-rework-arguments-validator-docs.md`.
- `combining-validation-results.adoc`
  - Document the additional `Validation` / `Validated` APIs:
    `Validation#bimap`, `map`, `mapError`, `mapErrors`, `orElseThrow`,
    `throwIfInvalid`; `Validated.successWith`, `Validated.failureWith`,
    `Validated.of`, `Validated.sequence`, `Validated.traverse`,
    `Validated.traverseIndexed`, `Validated.traverseOptional`.
  - `Validations.combine(...)` supports 2..16 validations.
  - Confirm the error type of `Validated#fold` is still
    `List<ConstraintViolation>` and that
    `ConstraintViolations#of(List)` still exists.
  - Remove any reference to `Either` support; it was dropped in 0.11.0 (the
    current docs may not mention it, so this is a check).
- Both files: verify every snippet against the 0.17.3 source. The
  `ApplicativeValidator` / `Validation` / `Validated` APIs were not renamed, but
  `andThen` semantics changed: `map` is the function-mapping method.

## Out of scope

- Argument-validator construction details (`004`).
- Constraint method lists (`006`-`008`).

## Verification

- All snippets compile against `am.ik.yavi:yavi:0.17.3`.
- The examples that describe accumulated violations still match the actual
  messages produced by 0.17.3 (run them).
- `grep -rn "Either\|Argument1Validator\|andThen(" src/main/asciidoc/combining-*.adoc`
  shows only intentional occurrences.
