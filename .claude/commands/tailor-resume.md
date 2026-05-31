# Tailor resume to job description

## Prerequisites

- `sources/` populated (or copy from `examples/sources/`).
- Job description in chat and/or `job-descriptions/*.md`.
- Output slug from user (e.g. `acme-senior-backend`) unless inferable from JD filename.

## Workflow

### 1. Confirm inputs

- If JD is only in chat, ask for **output slug** if not provided: `{company}-{role-slug}` in kebab-case.
- If user references a file under `job-descriptions/`, read that file and derive slug from filename when sensible.
- Create directory `output/{slug}/` if missing.

### 2. Load source materials

Read in order:

1. `sources/_index.md`
2. `sources/profile.md`
3. `sources/skills-inventory.md`
4. Every file under `sources/experience/` (newest first by Facts dates)
5. `sources/projects/`, `sources/education/` as needed

If `sources/` is missing, instruct user to copy `examples/sources/` → `sources/` and stop.

### 3. Parse job description

Build an internal checklist:

| Bucket | Content |
|--------|---------|
| `must_have` | Hard requirements (years, stack, domain) |
| `nice_to_have` | Preferred qualifications |
| `keywords` | Terms to mirror naturally in bullets |
| `responsibilities` | Role duties to map to experience |

Store in `resume.json` → `jd_analysis` (arrays of strings).

### 4. Match and plan (Experience focus)

For each experience file:

- Assign `relevance`: `high` | `medium` | `low`.
- Select 3–5 bullets (high), 2–3 (medium), 0–2 (low) from **Raw bullets** / **Metrics**, aligned to `must_have` first.
- Assign stable IDs: `exp_{start}_{company_slug}`, bullets `b1`, `b2`, …
- Plan `evidence_id` per bullet: `ev_{exp}_{b}` (e.g. `ev_nimbus_b1`).
- Do not reuse the same source fact in two bullets unless substantively different angles (rare).

Mark JD items with no source proof → `gaps[]` only.

### 5. Write `resume.json`

- Validate mentally against `.cursor/skills/tailor-resume/schema/resume.schema.json`.
- `meta`: `target_role`, `company`, `generated_at` (today), `language`: `"en"`, `output_slug`.
- `summary`: 2–3 sentences, English, only supported claims.
- `skills`: group into `core`, `tools`, `domains` — every item traceable via skills-inventory or experience Tools sections.
- `experience[]`: ordered reverse-chronological; each bullet has `text`, `evidence_id`, `skill_points`, `jd_tags`.
- `education[]`: from education sources only.
- `projects[]`: optional; only if JD-relevant.
- `gaps[]`: `{ requirement, status, note }`.

### 6. Write `evidence.md`

One section per bullet (and per project bullet if any). Follow the template at `.cursor/skills/tailor-resume/templates/evidence.md.template`

- Excerpt must be **verbatim** from source (≤2 sentences).
- `confidence: high` when excerpt directly supports claim; `medium` only for careful paraphrase.

### 7. Write `resume.md`

Render human-readable resume from JSON. Follow the template at `.cursor/skills/tailor-resume/templates/resume.md.template`

- No evidence sections in this file.
- ~1 page.

### 8. Self-check

- [ ] Every `experience[].bullets[].evidence_id` appears in `evidence.md`
- [ ] No bullet cites technology or metric absent from sources
- [ ] All `must_have` gaps listed in `gaps[]`, not smuggled into bullets
- [ ] English, consistent tense (past for ended roles, present for current)

### 9. Report to user

Summarize:

- Output path `output/{slug}/`
- Top 3 JD matches
- Roles de-emphasized or omitted
- Full `gaps` list
- Suggested `sources/` additions

## Example invocation

```
/tailor-resume

Slug: acme-senior-backend

[paste JD or reference job-descriptions/demo-acme-senior-backend.md]
```

## Do not

- Skip `evidence.md`
- Output only chat text without writing the three files
- Fabricate content (see `CLAUDE.md` → Resume Integrity)
