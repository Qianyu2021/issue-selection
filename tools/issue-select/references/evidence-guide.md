Evidence guide for `issue-select`

When grading candidate issues, the skill looks for the following evidence items in the
issue text, linked files, and repository context:

- Reproduction steps or example commands: explicit steps, stack traces, or failing tests.
- Small code pointers: a single file and a few lines referenced, or a reproducible snippet.
- Acceptance criteria: a concise statement of what success looks like for the change.
- Assignment status: whether the issue shows an assigned author, assigned PR, or mention
  of a contributor working on it.

If these items appear in linked files (e.g., a failing test file), the skill considers
them equivalent to inline evidence and records the matched source path.
