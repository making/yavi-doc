# Update Kotlin support and integration documentation

Difficulty: Medium

Dependencies: none. The Kotlin section cross-references the temporal and enum
constraints documented in `007` and `008`, but the edits are independent.

## Goal

Bring the Kotlin and integration sections in
`src/main/asciidoc/using-yavi.adoc` (`=== Kotlin Support`) and
`src/main/asciidoc/integrations.adoc` up to YAVI 0.17.3.

## Kotlin support

- The DSL (`validator<T> { ... }`) now has property overloads for
  `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`,
  `OffsetDateTime`, `Instant`, `Year`, `YearMonth`, `BigInteger`,
  `BigDecimal`, collections, maps, object arrays, primitive arrays and enums.
  Add examples for at least the temporal and enum overloads; do not enumerate
  every type.
- Document `onCondition { ... }`, `onGroup(group) { ... }` and `unwrap()`.
- Document `nestIfPresent`, `forEachIfPresent` (collection, map, array) which
  the current page does not show.
- Note the Kotlin DSL requires `kotlin-reflect`.
- Coroutine extensions (already present in 0.9.1 but absent from the docs):
  `am.ik.yavi.fn.ValidationExtensions` provides `awaitFold`, `awaitBimap`,
  `awaitMap`, `awaitMapErrors` and `awaitMapError`. Add a short async example
  and state that they require `kotlinx-coroutines-core`.
- Keep the existing basic/`forEach`/`nest` examples if they still compile;
  verify against the 0.17.3 DSL.

## Integrations

- `am.ik.yavi.core.BiValidator` was removed in 0.16.0. Replace the
  `=== Obtaining a BiValidator` section with `BiConsumer`:
  - `Validatable#toBiConsumer(ErrorHandler<E>)` and
    `ValueValidator#toBiConsumer(ErrorHandler<E>)`.
  - `ErrorHandler#handleError(E errors, String name, String messageKey,
    Object[] args, String defaultMessage)`.
  - `am.ik.yavi.factory.BiConsumerFactory` instead of `BiValidatorFactory`;
    `new BiConsumerFactory<>(errorHandler)` or
    `new BiConsumerFactory<>(separator, messageFormatter, errorHandler)`.
  - Show the Spring `Errors`/`BindingResult` shape, e.g. a lambda calling
    `rejectValue(name, messageKey, args, defaultMessage)`.
- Check the existing Spring MVC, WebFlux, `MessageSource` and
  `ValidatorFactory` examples against 0.17.3. The `applicative()` /
  `Validated#fold` usage should still compile; fix any other renamed API.
- `ViolationDetail` and `MessageSourceMessageFormatter` are unchanged; keep
  them.

## Verification

- Kotlin snippets compile with the Kotlin version used by YAVI 0.17.3
  (`kotlin.version` in `../yavi/pom.xml`), with `kotlin-reflect` and
  `kotlinx-coroutines-core`.
- Java integration snippets compile against `am.ik.yavi:yavi:0.17.3`.
- `grep -rn "BiValidator" src/main/asciidoc` returns nothing.
