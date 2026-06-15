---
name: feedback-separate-concept-files
description: Each distinct concept should get its own .md file in docs/concepts/, not be crammed into one file
metadata:
  type: feedback
---

When creating documentation for concepts, create a separate `.md` file per distinct concept inside `docs/concepts/`.

**Why:** User prefers granular, focused files — one concept per file — so each doc is easy to scan and reference individually.

**How to apply:** If asked to document multiple unrelated concepts, create one file per concept rather than one large combined file. Related sub-topics (e.g., named params and arrow functions both under Functions) can stay in the same file, but separate top-level concepts (e.g., Navigation vs State Management) go in separate files.