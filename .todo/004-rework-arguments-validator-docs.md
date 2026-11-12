# Rework the arguments-validator documentation for 0.17.3

Difficulty: High

Dependencies: none. It cross-references
`005-update-combining-validators-docs.md` for `split`/`combine`, but can be
completed independently.

## Goal

Rewrite `src/main/asciidoc/validating-arguments.adoc` for YAVI 0.17.3 and add a
"Always Valid model" tutorial to `src/main/asciidoc/getting-started.adoc`.

## Required content

- Introduce the current entry point:
  `am.ik.yavi.validator.Yavi#arguments()` returning
  `am.ik.yavi.builder.YaviArguments`, for example:

  ```java
  Arguments3Validator<String, String, Integer, Person> validator = Yavi.arguments()
      ._string("name", c -> c.notBlank().lessThanOrEqual(100))
      ._string("email", c -> c.notBlank().lessThanOrEqual(100).email())
      ._integer("age", c -> c.greaterThanOrEqual(0).lessThan(200))
      .apply(Person::new);
  ```

  Show `_localDate`, `_localTime`, `_localDateTime`, `_offsetDateTime`,
  `_instant`, `_year`, `_yearMonth`, `_zonedDateTime`, `_enum`, `_objectArray`,
  `_collection` and `_map` as available argument kinds.
- Keep the existing `ArgumentsValidatorBuilder.of(...).builder(...)` form for
  instance methods (`ArgumentsValidatorBuilder.of(userService::createUser)`)
  and for annotation-processor meta constants. Explain when each form is
  appropriate.
- `Arguments1Validator` .. `Arguments16Validator`; the maximum arity is 16.
- Single-value validators: `StringValidatorBuilder`, `IntegerValidatorBuilder`,
  etc. still exist; `build(Function)` produces a mapped validator. Replace
  `andThen(name -> new Name(name))` with `map(...)` and note that
  `andThen(Function)` is deprecated since 0.17.0. Keep `andThen(ValueValidator)`
  for chaining validators.
- Document the following APIs with short examples:
  - `compose(Function)` (source object -> argument)
  - `wrap()` / `unwrap(...)` and `lazy()` (including why `lazy()` exists)
  - `indexed(int)` for collection-element violation names
  - `liftList()`, `liftSet()`, `liftCollection(Supplier)`, `liftOptional()`
  - `split(...)` / `combine(...)` (cross-reference `005`)
  - `ArgumentsValidators.traverseN(...)` / `sequenceN(...)` for validating
    each element/result of a collection
- `ArgumentsN` value containers:
  - `Arguments1` .. `Arguments16`, `Arguments.of(...)`
  - `argN()`, `firstN()` / `lastN()`, `append`, `prepend`, `reverse`,
    `equals`/`hashCode`
  - `Arguments` is final and `ArgumentsN` no longer inherits `ArgumentsM`
    (0.16.0); do not describe an inheritance relationship.
- Add the "Always Valid model" pattern to `getting-started.adoc`:
  - private constructor plus a static `of(...)` factory returning
    `Validated<Car>` built with `Yavi.arguments()`
  - record with a compact constructor calling
    `validator.lazy().validated(...)`, explaining that `lazy()` prevents
    recursive construction / `StackOverflowError`
  - show how violations are inspected (`validated.errors()` /
    `ConstraintViolationsException#violations()`).
  The README in `../yavi` at 0.17.3 already contains a long version of this;
  condense it, do not copy it verbatim.
- Update the stale line `Argument1Validator<HttpServletRequest, Name>` to
  `Arguments1Validator<...>` in `combining-validators.adoc` while you are
  cross-referencing it.

## Out of scope

- Built-in constraint method lists (`006`-`008`).
- Kotlin coroutine extensions (`009`).

## Verification

- All snippets compile against `am.ik.yavi:yavi:0.17.3`.
- No `andThen(Function)` remains in the edited files.
- The "Always Valid model" snippets are covered by an actual run:
  invalid arguments return `Validated` with violations, valid arguments produce
  the object, and the record constructor throws
  `ConstraintViolationsException` for invalid input.
