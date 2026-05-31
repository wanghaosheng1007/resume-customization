# Resume Customization — Claude Code Rules

This project tailors English resumes to job descriptions using only facts from `sources/`.
Run `/tailor-resume` to start the workflow.

---

## Resume Integrity

You are tailoring a resume for a real person. **Trust and accuracy outweigh keyword stuffing.**

### Source of truth

- Use **only** facts from `sources/` (and `examples/sources/` only if the user has not created `sources/` yet).
- Read `sources/_index.md` and `sources/skills-inventory.md` before writing.
- JD requirements with **no** supporting source material → `gaps[]` in `resume.json`, **not** in resume body bullets.

### Forbidden

- Inventing or altering: company names, job titles, employment dates, education, certifications, awards.
- Inventing projects, responsibilities, or technologies the person did not document.
- Inflating metrics beyond what appears in `sources/` (paraphrase OK; rounding OK only if immaterial).

### Allowed

- Rewording bullets for clarity and JD alignment (same underlying fact).
- Selecting which documented bullets to include; omitting low-relevance roles or bullets.
- Combining related facts from the **same** role into one bullet if each fact is still traceable.

### Evidence (mandatory)

For **every** `experience[].bullets[]` and **every** `projects[]` bullet in `resume.json`:

1. Set `evidence_id` (e.g. `ev_b1`).
2. Add a matching section in `output/{slug}/evidence.md` with:
   - `source_file` path under `sources/`
   - `source_excerpt` — verbatim quote, ≤2 sentences from that file
   - `skill_points` and `jd_requirements_addressed`
   - `confidence`: `high` (direct quote supports claim) or `medium` (clear paraphrase only)

Before finishing, verify every `evidence_id` in JSON exists in `evidence.md`.

### Experience section priority

- Prefer 3–5 bullets per high-relevance role, 2–3 for medium, 0–2 for low (or omit role if irrelevant).
- Do not duplicate the same accomplishment across two roles.
- English only; action verbs; quantify when sources provide numbers.

### User communication

After generation, briefly report: strong JD matches, omitted material, all `gaps`, and suggested additions to `sources/`.

---

## Source File Layout

### Directory roles

| Path | Purpose |
|------|---------|
| `sources/profile.md` | Contact info and one-line positioning (Facts section) |
| `sources/_index.md` | Human-maintained index of all source files |
| `sources/skills-inventory.md` | Skills + pointers to proof files |
| `sources/experience/*.md` | One file per job; primary input for tailoring |
| `sources/projects/*.md` | Projects (employment or side) |
| `sources/education/*.md` | Degrees and academic facts |

### Required sections per experience file

Each `sources/experience/YYYY-MM_company_role.md` must include:

1. **Facts** — immutable: company, title, location, dates, team context.
2. **Tools** — technologies actually used.
3. **Metrics** — numbers only if true (latency, scale, %, counts).
4. **Raw bullets** — verbatim notes; agent may rewrite for resume but not contradict.

### Filename convention

`YYYY-MM_company_slug_role-slug.md` (start date of role).

### Reading order for tailor workflow

1. `sources/_index.md`
2. `sources/profile.md`, `sources/skills-inventory.md`
3. All `sources/experience/*.md` (sort by date descending)
4. `sources/projects/*.md`, `sources/education/*.md` as needed for JD

### Formats

- Prefer Markdown. Plain `.txt` is acceptable.
- Do not parse binary PDFs in v1; ask user to paste excerpts into `.md` if needed.

### When facts conflict

Prefer **Facts** section over **Raw bullets**. Flag conflict to user; do not guess.

---

## Output Schema and Files

Each JD run writes to `output/{slug}/` where `{slug}` is lowercase kebab-case (e.g. `acme-senior-backend`).

### Required artifacts

| File | Role |
|------|------|
| `resume.json` | Canonical structured resume; must validate against schema |
| `resume.md` | One-page English resume for humans (no evidence blocks) |
| `evidence.md` | Proof-of-evidence for every bullet |

### JSON schema

Follow `.cursor/skills/tailor-resume/schema/resume.schema.json`

Key rules:

- `meta.language` is always `"en"`.
- `meta.generated_at` is ISO 8601 date (YYYY-MM-DD).
- Every bullet has unique `id`, `evidence_id`, `skill_points[]`, `jd_tags[]`.
- `gaps[]` lists unmet JD requirements; `status` is `missing` or `partial`.

### evidence.md section template

Use exactly this structure per evidence entry:

```markdown
## {evidence_id} — {experience_id} / {bullet_id}

**Resume bullet:** {full bullet text}

**Source:** `sources/...`

**Excerpt:**
> {verbatim excerpt}

**Skill points demonstrated:** {comma-separated}

**JD requirements addressed:** {must_have / nice_to_have / keyword references}

**Confidence:** high | medium
```

### resume.md structure

1. Name and contact (from `sources/profile.md` Facts)
2. **Summary** (2–3 sentences)
3. **Skills** (grouped: core, tools, domains)
4. **Experience** (reverse chronological; company, title, dates, location, bullets)
5. **Education** (brief)
6. Optional **Projects** only if JD-relevant and documented

Keep to ~1 page unless user requests otherwise.

### Templates

See `.cursor/skills/tailor-resume/templates/resume.md.template` and `.cursor/skills/tailor-resume/templates/evidence.md.template`
