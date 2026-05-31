# Resume Tailor Agent

This workspace is a **resume customization kit** for Cursor Agent. It tailors English resume content to a job description (JD) using only facts from `sources/`.

## Quick start

1. Add your real experience under `sources/` (see `sources/experience/` for the file format).
2. Open **Agent** chat, paste the JD (or reference `@job-descriptions/your-file.md`).
3. Run **`/tailor-resume`** and provide an output slug (e.g. `acme-senior-backend`).
4. Review `output/{slug}/evidence.md` — every bullet must trace to a source file.
5. Use `output/{slug}/resume.md` for applications.

## What the agent will do

- Read all materials under `sources/`
- Prioritize and rewrite the **Experience** section for the JD
- Write `resume.json`, `resume.md`, and `evidence.md` under `output/{slug}/`

## What the agent must NOT do

- Invent companies, titles, dates, degrees, certifications, or metrics
- Claim skills or projects not supported by `sources/`
- Put JD requirements you cannot prove into the resume body (those go in `gaps` in JSON)

## Privacy

By default, `sources/` and `output/` are gitignored. Commit only `.cursor/`, `AGENTS.md`, and `README.md` if sharing the rule set publicly; keep personal data in a private repo or locally.
