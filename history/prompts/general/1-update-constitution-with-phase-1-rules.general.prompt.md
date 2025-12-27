---
id: 1
title: Update Constitution with Phase 1 Rules
stage: general
date_iso: 2025-12-23
surface: agent
model: claude-3-5-sonnet
feature: none
branch: 2-docusaurus-styling-overhaul
user: 
command: update_constitution_with_rules
labels: ["constitution", "rules", "phase-1"]
links:
  spec: null
  ticket: null
  adr: null
  pr: null
files_yaml:
  - .specify/memory/constitution.md
tests_yaml: []
prompt_text: |
  Update `.specify/memory/constitution.md` with these strict rules for Phase 1 (Book Creation):

  1. **Project Focus**:
     - Currently focusing ONLY on `/book_source` (Docusaurus).
     - Backend logic is paused.

  2. **Content Standards**:
     - All content MUST be written in `book_source/docs/`.
     - File format: Markdown (`.md`) or MDX (`.mdx`).
     - Tone: Professional, Engineering-focused, and Instructional.
     - Structure: Every chapter must have an Introduction, Core Concepts, and Code Examples.

  3. **Docusaurus Config**:
     - Sidebar must be auto-generated or explicitly defined in `sidebars.ts`.
     - Use standard Docusaurus classic preset.
response_text: |
  The `constitution.md` file has been successfully updated and committed.
  I have completed the user's request.
outcome: Success
evaluation: The constitution file was updated as requested, and the changes were committed.
---
