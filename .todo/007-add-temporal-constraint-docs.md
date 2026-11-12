# Add the temporal constraint documentation

Difficulty: High

Dependencies: none. This todo edits `built-in-constraints.adoc` in the same
file as `006` and `008`; coordinate concurrent edits. The Kotlin cross-reference
is completed in `009`.

## Goal

`src/main/asciidoc/built-in-constraints.adoc` has no temporal section at all,
although YAVI 0.10.0-0.17.3 ships temporal constraints. Add a `=== Temporal`
section (place it after the numeric section and before `=== Boolean`) with the
same example/table structure as the other sections.

## Types to cover

Constraint classes and corresponding builder shortcuts:

- `LocalDateConstraint` / `_localDate`
- `LocalTimeConstraint` / `_localTime`
- `LocalDateTimeConstraint` / `_localDateTime`
- `OffsetDateTimeConstraint` / `_offsetDateTime`
- `InstantConstraint` / `_instant`
- `YearConstraint` / `_year`
- `YearMonthConstraint` / `_yearMonth`
- `ZonedDateTimeConstraint` / `_zonedDateTime`

Also mention that `TemporalConstraintBase` supports custom
`TemporalAccessor` types through `ChronoLocalDateConstraintBase`,
`ChronoLocalDateTimeConstraintBase` and `ChronoZonedDateTimeConstraintBase`,
and that the Kotlin DSL has matching property overloads (cross-reference the
Kotlin section).

## Methods and semantics

For each type, the common methods come from
`../yavi/src/main/java/am/ik/yavi/constraint/base/TemporalConstraintBase.java`:

- `past()`, `pastOrPresent()`, `future()`, `futureOrPresent()` and the `Clock`
  overloads. Explain that the `Clock` overload exists for deterministic tests.
- `before(Supplier<V>)`, `beforeOrEqual(Supplier<V>)`,
  `after(Supplier<V>)`, `afterOrEqual(Supplier<V>)`, `between(Supplier<V>,
  Supplier<V>)` and the `(..., boolean memoize)` overloads. The fixed-value
  variants were removed; arguments are always suppliers.
- `fieldPredicate(TemporalField, LongPredicate)`.
- `memoize` defaults to `true`: the supplier is called at most once and the
  same value is used for comparison and message. Document the difference when
  `memoize` is `false`.
- `between` is exclusive on both ends and throws `IllegalArgumentException`
  when `rangeFrom` is after `rangeTo` at validation time.
- A `null` value is valid and skips the temporal checks.
- All types reject values that are equal for `before`/`after`; use the
  `OrEqual` variants for inclusive checks.

## Message keys

Document these keys with their arguments from
`../yavi/src/main/java/am/ik/yavi/core/ViolationMessage.java`:

- `temporal.past`, `temporal.pastOrPresent`, `temporal.future`,
  `temporal.futureOrPresent`
- `temporal.before`, `temporal.beforeOrEqual`, `temporal.after`,
  `temporal.afterOrEqual`, `temporal.between`, `temporal.field`

## Verification

- Every snippet compiles against `am.ik.yavi:yavi:0.17.3`.
- Exercise each method (including `past`, `future`, `between` with both
  `memoize` values, and `fieldPredicate`) in a scratch project and compare the
  messages with the tables.
- Check the generated HTML for the new section (after `002`).
