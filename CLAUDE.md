# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **detail.design skill** — a documentation-only repository containing curated UI design detail guidelines for AI coding assistants. There is no build system, no tests, and no compiled code. All content is Markdown. All content comes from https://detail.design.

Install: `npx skills add detaildotdesign/skill`

## Repository Structure

-   `skills/SKILL.md` — Main skill definition with metadata (name, description, license) in YAML frontmatter, plus the core philosophy and usage instructions
-   `skills/details/*.md` — 11 category-specific guideline files, each containing numbered rules with examples

## How the Skill System Works

`SKILL.md` is the entry point. It (1) resolves the request into an operating mode — **Build**, **Polish**, or **Review** — (2) routes the assistant to load only the matching file(s) from `details/`, (3) runs each candidate detail through a **decision gate** (serve the user, frequency of exposure, respect the user, consistency), and (4) for Review mode defines a fixed `file:line` + rule-ID output format. Each detail file is self-contained and follows a consistent format.

## Content Conventions

-   Each chapter file opens with a one-line scope sentence and a `**Skip when:**` note (when these details become over-engineering for that category)
-   Each rule is a `###`-level heading with a number prefix (e.g., `### 1. Title`). The citation ID is `<category>-<N>` — the chapter filename plus the rule number (e.g., `motion-12`, `form-3`)
-   Rule numbering is sequential within each file and **stable**: append new rules at the end; do not reorder
-   Each rule body is **observable/checkable** (not subjective) and carries a short code example where one clarifies the technique (HTML/CSS/JS/React); conceptual rules may stay prose-only
-   Rules should be **reusable techniques** applicable beyond a single product, **too subtle to notice individually**, or **easter eggs/moments of delight**
-   Rules should NOT be core features, onboarding/hero/card design, or pure decoration
-   The five core principles: craft over decoration, ambient over obvious, physics over magic, intent over input, consistency over novelty

## When Editing Content

-   Keep rules actionable; include a short code example where one makes the rule actionable
-   New detail categories need a row in the **routing table** of `SKILL.md` and must follow the chapter template (scope line, `Skip when`, numbered rules)
-   Rule numbering is sequential and stable within each file
