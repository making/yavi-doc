# Update the annotation-processor and introduction documentation

Difficulty: Medium

Dependencies: none. The introduction links to feature sections documented in
`006`-`009`, but the edits are independent.

## Goal

Check `src/main/asciidoc/annotation-processor.adoc` and
`src/main/asciidoc/introduction.adoc` against YAVI 0.17.3 and update them.

## Annotation processor

- YAVI deprecated the annotation processor in 0.12.0 and restored it in 0.12.3.
  The 0.17.3 source still contains `am.ik.yavi.meta.ConstraintTarget`,
  `ConstraintArguments`, `ConstraintMeta` and the processor in
  `am.ik.yavi.processor`; the docs are therefore still relevant. Verify the
  wording does not imply deprecation anywhere and that the setup instructions
  (running `mvn clean compile`) are complete. If the processor needs to be
  declared explicitly in a modern build, add a Maven `annotationProcessorPaths`
  example; check what YAVI itself does in `../yavi/pom.xml`.
- Regenerate the sample output shown in the page (the `_UserFormMeta`,
  `_UserArgumentsMeta`, `_UserServiceCreateUserArgumentsMeta` snippets) using
  the 0.17.3 processor, and update the snippets if the generated code or
  formatting changed.
- Document the supported meta types. The current `am.ik.yavi.meta` package
  contains `StringConstraintMeta`, `BooleanConstraintMeta`,
  `CharacterConstraintMeta`, byte/short/int/long/float/double,
  `BigIntegerConstraintMeta`, `BigDecimalConstraintMeta`, `ObjectConstraintMeta`
  and temporal metas (`LocalDate`, `LocalTime`, `LocalDateTime`,
  `OffsetDateTime`, `Instant`, `Year`, `YearMonth`, `ZonedDateTime`).
  There is no enum meta; say so or omit it.
- Verify the `@ConstraintTarget` getter / constructor / field-access variants
  and the `@ConstraintArguments` constructor / method variants still match the
  processor behavior.

## Introduction

- Update the feature list to reflect 0.10.0-0.17.3: temporal constraints,
  enum constraints, Kotlin coroutine integration, the "Always Valid model"
  approach, constraint groups/contexts and conflict strategy. Keep it short;
  this is a feature list, not a tutorial.
- Keep the link to
  `https://github.com/making/yavi/blob/develop/docs/FromBeanValidationToYAVI.md`.
  That guide was updated for the URL protocol/host/port additions; no change is
  required here, but check the link still resolves.

## Verification

- Generate a sample with the processor and confirm the snippets in the page
  match the generated files.
- `grep -rn "deprecat" src/main/asciidoc/annotation-processor.adoc` only shows
  intentional notes.
- All snippets compile/run in a scratch project.
