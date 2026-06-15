---
name: feedback-markdownlint
description: User has markdownlint installed and wants all markdown files to comply with its rules
metadata:
  type: feedback
---

Always write markdown that passes markdownlint. Specifically:

- No `---` thematic breaks as section dividers between headings (linter strips them)
- Blank lines around headings, code blocks, and lists (MD022, MD031, MD032)
- Consistent heading style (ATX — use `#`, not underline style)
- No trailing spaces
- No multiple consecutive blank lines

**Why:** User has the markdownlint VSCode extension installed and wants it to validate their docs cleanly.

**How to apply:** Every `.md` file created or edited in this project — write lint-clean markdown from the start so the linter doesn't have to fix it.