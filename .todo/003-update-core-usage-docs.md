# Update the core usage documentation for 0.17.3

Difficulty: Medium

Dependencies: none. This todo and `005-update-combining-validators-docs.md`
both edit `combining-validation-results.adoc`; coordinate the edits if they are
worked on concurrently.

## Goal

Bring `src/main/asciidoc/using-yavi.adoc` (and the core parts of
`combining-validation-results.adoc`) in line with the YAVI 0.17.3 API. Use
`../yavi` at tag `0.17.3` as the source of truth; the Javadoc in the source is
authoritative.

## Stale content to fix

- The group/conditional sections describe `ConstraintGroup` only. Since 0.11.0
  `ConstraintCondition` receives a `ConstraintContext`, and `ConstraintGroup`
  is one kind of context. Document:
  - `ConstraintContext.from(Map)` / `ConstraintContext.from(Function)` and
    reading attributes with `context.attribute("key").value(...)`.
  - `ConstraintGroup.of("CREATE")` as well as the existing enum pattern; a
    group matches by name and `ConstraintGroup.DEFAULT` is used when none is
    passed.
  - `validator.validate(target, context)`.
- Add `conflictStrategy` documentation:
  `ValidatorBuilder#conflictStrategy(ConflictStrategy)` with
  `ConflictStrategy.NOOP` (default) and `OVERRIDE`, and how a custom strategy
  can mutate the existing predicate list. Explain that it applies when the same
  constraint name is registered more than once.
- Fail-fast: state that nested, collection and conditional validators honor the
  parent's fail-fast mode since 0.13.0 (previously they always accumulated), and
  that `Validatable#failFast(boolean)` / `isFailFast()` exist. Keep the existing
  section and extend it rather than adding a duplicate.
- `ValueValidator` / `Validatable`:
  - `andThen(Function)` is deprecated since 0.17.0; use `map(Function)`.
    `andThen(ValueValidator)` (chaining validators) is still supported.
  - `validated(...)` throws `ConstraintViolationsException`; mention it and
    `Validation#throwIfInvalid` / `ConstraintViolations#throwIfInvalid`.
  - `Validatable.from(ValueValidator)` (0.11.0) converts a value validator
    into a core validator, discarding the produced value.
- `ConstraintViolation`: mention `ConstraintViolation.builder()` (0.15.0) with
  its staged name/messageKey/defaultMessageFormat/args/messageFormatter/locale
  methods, and that overriding a message preserves the violated value
  (0.17.2).
- `SimpleMessageFormatter`: mention `SimpleMessageFormatter.getInstance()` and
  the public no-arg constructor; keep the custom `MessageFormatter` example.
- Custom constraints: verify the `CustomConstraint` example still matches
  (`arguments(V violatedValue)` and `messageKey()` / `defaultMessageFormat()`
  are current); mention `ViolationMessage` and `ViolatedValue` where useful.
- Remove any remaining use of removed APIs (`BiValidator`, `ValidatorSubset`,
  `Either`, `ValidatorBuilder#clone`, `CharSequenceConstraint#isLocalDate`).
  `using-yavi.adoc` itself may not use them; check cross-references.

## Out of scope

- The argument-validator APIs are covered by `004`, combining by `005`, and
  built-in constraints by `006`-`008`. Keep this todo to core validator usage
  and result handling.

## Verification

- Every Java snippet compiles against `am.ik.yavi:yavi:0.17.3` (compile the
  snippets in a scratch project outside this repository, or use jshell with the
  jar on the classpath).
- No reference to a removed/deprecated API remains in the edited sections.
- `grep -n "andThen" src/main/asciidoc/using-yavi.adoc` only shows the
  validator-chaining form or an explicit note that the function form is
  deprecated.
