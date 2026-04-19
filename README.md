# Skills Lab

Quick reference for the skills in this repository.

## Install (General)

```bash
npx skills add PatricioOsorio/skills-lab
```

## Available Skills

### agents-setup
Sets up or repairs a single AI-rules source of truth using `AGENTS.md`, and points satellite files to it.

Install:
```bash
npx skills add PatricioOsorio/skills-lab --skill agents-setup
```

Use when:
- You want to consolidate AI instructions in one place.
- Your project has duplicated or inconsistent agent rules.

Example prompt:
"Set up AGENTS.md as the source of truth for this project and wire Claude/Copilot files to it."

### bem-styling-standards-angular
Defines Angular styling conventions using semantic blocks + BEM initials, with Tailwind CSS 4 and DaisyUI 5 applied in component CSS.

Install:
```bash
npx skills add PatricioOsorio/skills-lab --skill bem-styling-standards-angular
```

Use when:
- You are creating or refactoring Angular UI components.
- You need consistent, maintainable BEM naming and cleaner templates.

Example prompt:
"Refactor this Angular component to follow the BEM styling standards skill, moving utility-heavy HTML classes into component CSS."

### prompt-improver
Improves vague prompts into structured, production-ready prompts using prompt-engineering frameworks and concise planning interviews.

Install:
```bash
npx skills add PatricioOsorio/skills-lab --skill prompt-improver
```

Use when:
- A prompt is unclear or gives inconsistent model outputs.
- You want better structure, constraints, and output quality.

Example prompt:
"Improve this prompt for a coding task and return a cleaner, more reliable version with explicit output format."

## Folder Structure

- skills/agents-setup/SKILL.md
- skills/bem-styling-standards-angular/SKILL.md
- skills/prompt-improver/SKILL.md