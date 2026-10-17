# Changelog fragments

This directory holds one small file per change. At release time the
release build reads every fragment, groups the fragments by type, and
writes them into `CHANGELOG.md` as the section for the new version. The
build then deletes the fragment files.

Do not edit `CHANGELOG.md` to describe a change. Add a fragment instead.
One file per change means two pull requests never edit the same lines,
so parallel pull requests cannot conflict on the changelog.

## File name

Name the file after the issue the change closes, or after the pull
request itself when no issue exists:

```
<issue>.<type>.md
```

Example: `123.fixed.md`.

The type must be one of the changelog sections:

| Type         | Use it for                                   |
| ------------ | -------------------------------------------- |
| `added`      | A new feature or capability                  |
| `changed`    | A change to existing behavior                |
| `deprecated` | A feature that will be removed in a release  |
| `removed`    | A feature or code path that is gone          |
| `fixed`      | A bug fix                                    |
| `security`   | A security fix or hardening change           |

Two fragments for the same issue: add a counter before the extension,
for example `123.fixed.md` and `123.fixed.2.md`. Both entries render
under the same issue reference.

A change with no issue and no pull request number yet: use an orphan
fragment. Start the name with `+`, for example `+docs-typo.fixed.md`.
Rename the file to the numbered form once the pull request exists, or
leave it; orphan fragments render without an issue reference.

## File content

Write the entry text only:

- Use the style of the existing entries in `CHANGELOG.md`. State what
  changed and, when it matters, why.
- Do not add the `(#123)` reference. The release build appends the
  reference from the file name.
- Do not add a bullet marker. The release build adds the `- ` marker.
- Keep the entry to the change itself. One fragment, one change.

Example, file `123.fixed.md`:

```
The discover command no longer prints a negative age for an issue with
a future update timestamp. Ages below zero are clamped to zero.
```

The rendered changelog entry:

```
- The discover command no longer prints a negative age for an issue with
  a future update timestamp. Ages below zero are clamped to zero. (#123)
```

## Notes for maintainers

- Preview a release section without writing it:
  `uvx towncrier@26.9.0 build --draft --version X.Y.Z --date YYYY-MM-DD`
- Build the section into `CHANGELOG.md`:
  `uvx towncrier@26.9.0 build --yes --version X.Y.Z --date YYYY-MM-DD`
- `README.md` and `.gitkeep` in this directory are ignored by the build.
  Every other file here must follow the naming rules above, or the build
  fails.
