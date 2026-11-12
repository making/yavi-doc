# Update numeric, enum, array, collection and object constraint docs

Difficulty: Medium

Dependencies: none. This todo edits `built-in-constraints.adoc` in the same
file as `006` and `007`; coordinate concurrent edits.

## Goal

Add or update the constraints in `src/main/asciidoc/built-in-constraints.adoc`
that are missing or stale relative to YAVI 0.17.3.

## Numeric

- Add `positive()`, `positiveOrZero()`, `negative()`, `negativeOrZero()`
  (0.10.0) to the numeric section. They apply to
  `Byte`/`Short`/`Integer`/`Long`/`Float`/`Double`/`BigInteger`/`BigDecimal`.
- Message keys: `numeric.positive`, `numeric.positiveOrZero`,
  `numeric.negative`, `numeric.negativeOrZero`.
- Note that the misspelled `negaitveOrZero` method is gone; do not document it.

## Enum

Add a new `=== Enum` section:

- `ValidatorBuilder#_enum` / `constraint(toEnum, ...)` and
  `ArgumentsValidatorBuilder#_enum` / `YaviArguments#_enum`.
- `EnumConstraint#oneOf(E...)` (0.14.0) and `EnumConstraint` in general.
- `EnumValidatorBuilder` / `EnumValidator`.
- Explain that `oneOf(Enum...)` uses `object.oneOf` and rejects an empty
  argument list; `null` is valid unless `notNull()` is added.

## Array

Add a new `=== Array` section (it is currently absent):

- `ObjectArrayConstraint` and the primitive array constraints
  (`BooleanArrayConstraint`, `ByteArrayConstraint`, `CharArrayConstraint`,
  `ShortArrayConstraint`, `IntArrayConstraint`, `LongArrayConstraint`,
  `FloatArrayConstraint`, `DoubleArrayConstraint`).
- The builder shortcuts `_objectArray`, `_booleanArray`, `_byteArray`,
  `_charArray`, `_shortArray`, `_intArray`, `_longArray`, `_floatArray`,
  `_doubleArray`.
- Common container methods: `notNull`, `isNull`, `notEmpty`, `fixedSize`,
  `greaterThan`, `greaterThanOrEqual`, `lessThan`, `lessThanOrEqual`,
  `contains`.
- Message keys are the `container.*` and `array.contains` keys from
  `ViolationMessage.java`.

## Collection and Map

- Add `unique()` (0.8.3, missing from the docs). The violation includes the
  duplicated elements; message key `collection.unique`.
- Add `containsAll(Collection)` (0.14.3). It reports the missing elements and
  uses the same semantics for `Set` and non-`Set` collections; message key
  `collection.containsAll`.
- Explain that `forEach` / `forEachIfPresent` now validate `null` elements
  correctly (0.11.0); a null element is reported with the element index in the
  property name, and `notNull()` still applies.
- Map constraints are unchanged except for the shared base methods; verify the
  existing section against the source and add anything missing.

## Object

- Add `equalTo(Object)` (0.10.0), `oneOf(Collection)` (0.10.0) and
  `notOneOf(Collection)` (0.14.1), with message keys `object.equalTo`,
  `object.oneOf`, `object.notOneOf`.
- Keep the existing `password()` material.

## Verification

- Each new example compiles against `am.ik.yavi:yavi:0.17.3`.
- Run one valid and one invalid input per new method and compare the message
  with the table.
- Check the section anchors and the table of contents after the build (`002`).
