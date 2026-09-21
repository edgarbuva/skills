---
name: specs-generator
description: Turn a software requirement into user stories with acceptance criteria and a technical specification in docs/spec/. Use when the user asks for a spec, user stories, or requirement documentation before implementation.
---

# Generate a software specification

Create a specification from the user's requirement. Draft user stories first, then use them to define the scope and technical details of the SPEC. Write the deliverable in the language requested by the user; otherwise follow the language of their request.

## Understand the requirement

Read the request and relevant project context before drafting. Preserve stated behavior, constraints, terminology, and existing architecture. Ask a focused question only when missing information prevents a useful specification. Record reasonable assumptions and unresolved decisions in the SPEC instead of blocking on minor details. Do not treat inferred implementation choices as confirmed requirements.

## Draft user stories

Write small, independently valuable stories when the requirement supports them. Number them `US-01`, `US-02`, and so on. For each story, identify the actor, capability, and benefit, then provide testable acceptance criteria using Given/When/Then. Cover the main path and relevant alternate or failure paths; do not add artificial scenarios to meet a fixed count. For infrastructure work without an end-user actor, use the appropriate operator, developer, or system role.

Use this shape, translated to the deliverable language when appropriate:

```markdown
### US-01: Short title

As a <role>, I want <capability> so that <benefit>.

**Acceptance criteria**

- Given <context>, when <action>, then <observable result>.
```

If the user requested both stories and a SPEC, continue directly to the complete deliverable. Show an intermediate draft only when the user asked to review stories before the SPEC or a blocking ambiguity requires their input.

## Write the SPEC

Include the sections below when they contain useful information. Keep the user stories and acceptance criteria intact so each stated behavior remains traceable.

```markdown
# SPEC: <feature name>

**Date:** <current date>
**Status:** Draft

## Summary
<Purpose and expected outcome>

## Scope
<Included behavior and meaningful exclusions>

## User stories
<Stories and acceptance criteria>

## Nonfunctional requirements
<Only requirements supported by the request or project context>

## Data model
<New or changed data and relationships, if relevant>

## Technical considerations
<Relevant integrations, dependencies, constraints, and decisions>

## Assumptions and open questions
<Unconfirmed assumptions and decisions that remain open>
```

Omit inapplicable sections rather than filling them with generic text. Distinguish explicit requirements, project facts, and assumptions. Make exclusions explicit only when they clarify the requested scope. Keep technical proposals proportional to the available evidence; do not invent performance targets, security controls, schemas, or architecture decisions.

## Save and report

Identify the workspace root for the user's current project. Create `docs/spec/` under that root if it does not exist, even when the working directory is a subdirectory. Save the SPEC as `<workspace-root>/docs/spec/<NN>-<feature-slug>.md`. Use a short kebab-case slug. Inspect existing numbered specs in that directory and choose the next unused two-digit number without overwriting a file. If the user specified another location or naming scheme, follow it.

Report the file path, number of stories, and any open decision that could materially affect implementation.
