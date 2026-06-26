# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo
`thai-legal` = agent-skills collection. No app code, no build/lint/test. Upstream source of the
`legal-research-note` skill, shipped via GitHub + `skills-lock.json` content hash.

## Skill files — keep in sync
- Canonical: `skills/<name>/SKILL.md` (the `skillPath` in `skills-lock.json`).
- Mirror: `.agents/skills/<name>/SKILL.md` — must stay byte-identical to canonical.
- `computedHash` is tool-managed (not a plain `shasum`); never hand-edit — refresh via skills tooling.
- Author/edit skills through `/skill-creator`, not by hand.

## Answering
**Normal conversational Q&A → answer directly.** No verification swarm, no orchestration overhead.

**Skill / vault work only** — running `legal-research-note`, or building/updating `research/` — uses the 3-role
pipeline; the main agent orchestrates and does not write the note itself:
1. **Planner** — scope the topic; list every number/rate to verify.
2. **Generator** — research + draft the note.
3. **Verifier** — independent subagent re-checks every rate, section, and citation against live sources before
   the note is saved. Mandatory for vault notes, not done by the generator.

## research/ = wiki
Notes form an Obsidian-style wiki. Contract (full template in SKILL.md): one topic = one kebab-case file
(Thai allowed), update-in-place (never duplicate), fixed Thai template — frontmatter (`ค้นเมื่อ` real date,
`เสี่ยง` 🔴/🟡/🟢) then `## หลักกฎหมาย` / `## ใช้กับ 357 ยังไง` / `## ตัวเลข/อัตราล่าสุด` / `## ที่มา` / `## เกี่ยวข้อง`.
Rates are WebSearch-verified (never from memory); 🔴 carries a lawyer-review caveat; `[[wiki links]]` connect
topics. Company context = 357 Enterprise ("357").
