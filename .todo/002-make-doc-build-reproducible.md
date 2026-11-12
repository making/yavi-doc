# Make the documentation build reproducible

Difficulty: Medium

Dependencies: none. This should be completed before the verification steps of
`001` and `011` can pass.

## Problem

`./mvnw clean package` does not resolve dependencies:

```
Could not transfer artifact io.spring.docresources:spring-doc-resources:pom:0.2.5
from/to spring-release (https://repo.spring.io/release): Authentication failed ...
401 Unauthorized
```

- `https://repo.spring.io/release/.../spring-doc-resources-0.2.5.pom` returns 401.
- The artifact is not on Maven Central (404).
- The local `~/.m2` contains only a `.lastUpdated` marker, so the build cannot
  proceed.
- The zip is what supplies `css/spring.css`, `js/highlight`, etc. to the output
  directory. Without it the HTML still builds but is unstyled and the
  `source-highlighter` is broken.

## Direction

Pick the most durable option, in this order of preference:

1. **Vendor the required assets in this repository.** Extract the contents of
   `spring-doc-resources-0.2.5.zip` (obtain it from a working mirror or the
   Spring project once), keep only the files the docs actually reference
   (`css/spring.css`, the `js/highlight` theme, fonts/images if referenced, and
   anything else `docinfo-footer.html` needs), and remove the
   `spring-doc-resources` dependency and the `download-maven-plugin` execution.
   Configure `asciidoctor-maven-plugin`'s `stylesdir`/`highlightjsdir` (or a
   `maven-resources-plugin` copy into `docs/`) so the files land next to the
   generated HTML. This removes all network dependencies from the build.
2. If vendoring is too invasive, point the repository at a mirror that still
   serves 0.2.5. `https://repo.huaweicloud.com/repository/maven` returned 200
   for the POM; verify the zip too. Keep the mirror scoped to this artifact if
   possible.
3. Configure credentials for `repo.spring.io` (least reproducible; avoid if the
   credentials are not needed for anything else).

Check `src/main/asciidoc/docinfo-footer.html` and the generated HTML for the
exact asset paths before deleting anything. The maven wrapper is 3.6.3; upgrade
only if a plugin requires it, not as part of this task.

## Verification

- `./mvnw clean package` succeeds from a clean `~/.m2` for this artifact (or
  with the artifact removed locally), without credentials.
- `docs/index.html` references the bundled CSS/JS and opens with styles.
- `git status` is clean apart from the intended build files; `docs/` stays
  gitignored.
