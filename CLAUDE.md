# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **detail.design skill** — a documentation-only repository containing curated UI design detail guidelines for AI coding assistants. There is no build system, no tests, and no compiled code. All content is Markdown. All content comes from https://detail.design.

Install: `npx skills add detaildotdesign/skill`

## Repository Structure

-   `skills/SKILL.md` — Main skill definition with metadata (name, description, license) in YAML frontmatter, plus the core philosophy and usage instructions
-   `skills/details/*.md` — 11 category-specific guideline files, each containing numbered rules with examples

## How the Skill System Works

`SKILL.md` is the entry point. It directs the AI assistant to load specific files from `details/` based on what the user is working on (e.g., load `details/form.md` when working on forms). Each detail file is self-contained and follows a consistent format: numbered rules, each with a title, explanation, and often a code example.

## Content Conventions

-   Each rule in a detail file is a `###`-level heading with a number prefix (e.g., `### 1. Title`)
-   Rules should be **reusable techniques** applicable beyond a single product, **too subtle to notice individually**, or **easter eggs/moments of delight**
-   Rules should NOT be core features, onboarding/hero/card design, or pure decoration
-   The five core principles: craft over decoration, ambient over obvious, physics over magic, intent over input, consistency over novelty

## When Editing Content

-   Keep rules actionable and include code examples where possible (HTML/CSS/JS/React)
-   New detail categories need a corresponding entry in the "How to use this skill" section of `SKILL.md`
-   Rule numbering is sequential within each file
