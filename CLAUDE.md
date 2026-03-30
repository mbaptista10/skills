# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a collection of Claude Code skills — reusable slash commands that extend Claude Code's capabilities. Skills are installed via `npx skills add mbaptista10/skills` and become available as `/skill-name` commands.

## Repository Structure

Each skill lives in its own directory with a single `SKILL.md` file that defines the skill's metadata (frontmatter: name, description) and instructions (workflow, rules, examples).

## Current Skills

- **semantic-commits** (`/semantic-commits`): Analyzes staged files, groups them semantically, and creates separate conventional commits per group. Requires user confirmation before committing.
- **semantic-pr** (`/semantic-pr`): Creates/edits PRs by analyzing branch commits, collecting context from the user, and generating structured PRs with conventional commit prefix titles. Default language is PT-BR.

## Conventions

- Skills follow the SKILL.md format: YAML frontmatter (`name`, `description`) + markdown instructions with a strict step-by-step workflow.
- Both skills enforce **Conventional Commits** (`type(scope): description`) with imperative mood.
- Neither skill should include AI attribution or co-authorship metadata in generated commits/PRs.
- PR titles must not exceed 69 characters.
- Commit bodies require at least one bullet point describing the change.

## Adding a New Skill

Create a new directory with a `SKILL.md` file following the same frontmatter + instructions pattern. Update `README.md` to include the new skill in the table.
