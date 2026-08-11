# Summary: How to Write a Git Commit Message (chris.beams.io)

Source: https://chris.beams.io/git-commit (also at https://cbea.ms/git-commit/)

## Key Advice (The Seven Rules)

1. Separate subject from body with a blank line.
2. Limit the subject line to 50 characters (72 hard limit).
3. Capitalize the subject line.
4. Do not end the subject line with a period.
5. Use the imperative mood in the subject line (e.g. "Fix bug", not "Fixed bug").
6. Wrap the body at 72 characters.
7. Use the body to explain *what* and *why*, not *how* (code explains how).

## Why It Matters
- A good commit message communicates *why* a change was made (diffs only show *what*).
- Enables better use of `git log`, `blame`, `revert`, `rebase`, etc.
- Consistent style makes history readable and maintainable.

## Format Example
```
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. Explain the problem being solved and why.

Resolves: #123
See also: #456
```

## Additional Tips
- Prefer command line over IDE for Git operations.
- Keep commits atomic when possible.
- The subject should complete the sentence: "If applied, this commit will <subject>"

This summary should be used when writing or reviewing commit messages.