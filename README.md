# Resume Customization (Cursor Agent)

Tailor an English resume to a job description using Cursor **Rules** + **Skills**, with a full **evidence trail** for every experience bullet.

## Setup

1. Clone or copy this project.
2. Copy templates: `Copy-Item -Recurse examples\sources sources` (PowerShell) or `cp -r examples/sources sources`.
3. Replace example facts in `sources/` with real experience.
4. Open the folder in Cursor.

## Usage

| Step | Action |
|------|--------|
| 1 | Maintain raw materials in `sources/` |
| 2 | Paste JD in Agent chat or save to `job-descriptions/` |
| 3 | Run `/tailor-resume` |
| 4 | Audit `output/{company-role}/evidence.md` |
| 5 | Export `resume.md` to Word / LaTeX as needed |

## Outputs (per JD)

| File | Purpose |
|------|---------|
| `resume.json` | Structured resume (machine-readable) |
| `resume.md` | One-page human-readable resume |
| `evidence.md` | Proof: each bullet → source file + excerpt + skill points |

## Repository layout

```
.cursor/rules/          # Integrity and format guardrails
.cursor/skills/tailor-resume/   # Main workflow (/tailor-resume)
sources/                # Private experience materials (gitignored by default)
job-descriptions/       # Optional saved JDs
output/                 # Generated artifacts (gitignored)
```

## Sharing with your friend

- Share the repo **without** `sources/` and `output/` (they stay local).
- Your friend copies `sources/` templates and fills in real content locally.

## Demo run

A reference output for the sample JD lives at `output/demo-acme-senior-backend/` (local only; gitignored). Compare `resume.json`, `resume.md`, and `evidence.md` to see the expected evidence chain.

## 中文说明

本项目让 Cursor Agent 根据 JD 从 `sources/` 中的真实材料生成英文简历，并为每条经历要点提供 `evidence.md` 证据链。使用 `/tailor-resume` 触发工作流。首次使用请从 `examples/sources` 复制到 `sources/`。
