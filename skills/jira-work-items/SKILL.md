---
name: jira-work-items
description: Turn a rough idea, feature request, spec/PRD, meeting notes, or codebase context into a linked set of Jira work items — one epic, analysis items for the unknowns, and stories — with clear structure and acceptance criteria. Creates them in Jira through the Atlassian MCP when it is available, otherwise writes them as markdown files. Use this whenever the user wants to create, write, draft, split, or break down a Jira epic, story, analysis/spike, ticket, or backlog for a feature, even if they don't say "Jira work item" explicitly (e.g. "turn this spec into tickets", "write stories for this", "plan this feature in Jira").
---

# Jira work items

Turn unstructured input into a small, well-formed set of Jira items: an **epic**, the **analysis** items that resolve open questions, and the **stories** that deliver the work. The goal is tickets a teammate can pick up without asking what they mean.

## Workflow

### 1. Understand the input

The input may be a one-line idea, a spec or PRD (read the file if a path is given), meeting notes or a chat thread, or the current codebase. If the work touches code, look at the relevant code first so stories reflect what actually exists instead of guesses.

Ask the user only for what you cannot infer and that changes the output: the Jira project key, and whether the epic already exists (then only create children). Batch questions into one message. Do not interview at length; unclear points that don't block drafting become open questions inside an analysis item.

### 2. Decompose

- **One epic** for the overall outcome.
- **Analysis items** for anything unknown that would make a story impossible to estimate or build: unclear requirements, unproven technical approach, missing data, external dependencies. Each analysis ends in a decision or a document, not code. If nothing is unknown, don't invent analysis items.
- **Stories** as vertical slices: each delivers something a user or system can observe, and can be finished in a sprint or less. Prefer several small stories over a large one. Avoid splitting by technical layer ("backend", "frontend") unless the layers are owned separately.
- Note dependencies: which stories are blocked by which analysis item, and a sensible order.

### 3. Show the plan, then draft

Before writing full descriptions, show a short hierarchy (titles, types, blocked-by, order) and get a go-ahead. Restructuring a plan is cheap; rewriting twelve drafted tickets is not. Skip this confirmation only if the user asked for a single item.

### 4. Write each item using the templates below

Every item type starts with **Context**, so a ticket read in isolation months later still says why it exists. Section headings are `##`. The epic and analysis templates below are defaults until the team supplies examples like the story one.

**Epic**
- Context: the problem or opportunity and who is affected
- Goal: the outcome, in one or two sentences
- Scope: in scope / out of scope
- Success criteria: observable, ideally measurable
- Risks and dependencies
- Child items: the analysis items and stories, in order

**Analysis**
- Context: why this is unknown and what it blocks
- Questions to answer: a numbered list of specific questions
- Approach: how to find out (spike, prototype, talk to X, read Y)
- Timebox: how long to spend before reporting back
- Deliverable: what "done" produces (decision record, doc, recommendation)
- Out of scope

**Story** — the team's real example is `examples/story-example.md`; read it before drafting a story and match its level of detail and tone. Sections, in this order, as `##` headings:
- Context: the user-story line, "As a <who>, I want <what>", with the purpose folded into the sentence. One or two lines, no more.
- Description: the goal in plain language, what the consumer can do afterwards, and what this story deliberately does *not* cover (name the endpoint, screen or component that owns the excluded part). Also state security here: allowed roles, audience, scope.
- Technical Notes: the implementation contract, concrete enough to build from. For an API story that is method and path, request headers, response status codes, and an example JSON payload in a fenced block. For non-API stories use the equivalent (schema change, config keys, events). Use `###` subheadings such as Request and Response.
- Acceptance Criteria: a checklist, one line per behaviour, each starting with `[]` and written as a full Given / When / Then sentence, with all three parts even for checks about a response's shape (say "when they send the request, then the response is JSON…" rather than dropping the "when"). Cover the happy path, the authorization failure, and the shape of the response, so every status code and rule in the description has a criterion.

Keep names, roles and paths in backticks. Write specifics (real endpoint, real role names, realistic sample data) instead of placeholders; if the input doesn't give them, choose plausible ones and mark them as assumptions for the user to confirm.

Titles are short, start with a verb or the outcome, and make sense in a board column. Don't repeat the type in the title.

### 5. Deliver

**Try the Atlassian MCP first.** Look for Jira tools (use ToolSearch with a query like "atlassian jira create issue" if tools are deferred). If they exist:
1. Creating tickets is visible to the whole team and awkward to undo, so confirm the final list and the target project with the user before creating anything.
2. Create the epic first, then create analysis items and stories with the epic as parent, then add "blocks" links where the plan has dependencies.
3. Report the created keys and links as a hierarchy. If a call fails (unknown issue type, missing required field), say which item failed and why. Don't retry blindly, and don't silently drop items.

**Otherwise, write markdown files** (also the fallback if the MCP is present but the user doesn't want live creation):

```
jira-items/<epic-slug>/
  README.md            # hierarchy, order, dependencies
  00-epic-<slug>.md
  01-analysis-<slug>.md
  02-story-<slug>.md
```

Each file starts with frontmatter so it can be imported or pasted later:

```yaml
---
type: Story            # Epic | Analysis | Story
title: ...
parent: <epic title or key>
blocked_by: [<titles>]
labels: []
---
```

Then the body sections from the templates. Write the files in the current working directory unless the user names another location, and tell them where they are.

## Conventions

These defaults apply until the user says otherwise; if they describe their team's structure, follow theirs and mention any place you deviated from these:

- "Analysis" is treated as its own issue type. If the Jira project has no such type, use Task or Spike and add an `analysis` label.
- Acceptance criteria are Given / When / Then checklist lines starting with `[]`, as in the story example.
- Reference material for the team's style lives in `examples/`; when an `epic-example.md` or `analysis-example.md` appears there, prefer it over the default templates below.
- Write in the language of the input.
