---
name: skill-creator
description: "Guides creation and iteration of Claude skill packages — generates YAML frontmatter, structures markdown instructions, creates bundled resources (scripts, references, assets), and validates skill format via packaging. Use when creating a new skill, writing a SKILL.md, building a skill file, updating an existing skill, or teaching Claude a custom workflow."
license: Complete terms in LICENSE.txt
---

# Skill Creator

Guide for creating, editing, and packaging Claude skill packages. For background on skill anatomy, resource types, and progressive disclosure, see `references/skill-anatomy.md`.

## Example: Well-Written SKILL.md

```yaml
---
name: pdf-editor
description: "Rotates, merges, splits, and watermarks PDF files using bundled Python scripts. Use when editing a PDF, rotating pages, combining documents, or adding watermarks to a file."
---
```

```markdown
# PDF Editor

Manipulate PDF files via bundled scripts. Supports rotation, merging, splitting, and watermarking.

## Workflows

### Rotate pages
Run `scripts/rotate_pdf.py <input> <degrees> <output>` to rotate pages.

### Merge documents
Run `scripts/merge_pdfs.py <output> <input1> <input2> ...` to combine files.
```

## Skill Creation Process

Follow these steps in order. Skip a step only when clearly inapplicable.

### Step 1: Understand Usage with Concrete Examples

Skip only when usage patterns are already well understood.

Gather concrete examples of how the skill will be triggered. Ask targeted questions:

- "What functionality should this skill support?"
- "What would a user say that should trigger this skill?"
- "Can you give example prompts?"

Limit questions per message. Conclude when the required functionality is clear.

### Step 2: Plan Reusable Resources

For each example, identify what scripts, references, or assets would prevent repeated work:

| Pattern | Resource |
|---------|----------|
| Same code rewritten each time | `scripts/` executable |
| Same boilerplate files each time | `assets/` template |
| Same schemas/docs looked up each time | `references/` file |

### Step 3: Initialize the Skill

Skip if the skill already exists and only needs editing or packaging.

Run the initialization script to scaffold the skill directory:

```bash
scripts/init_skill.py <skill-name> --path <output-directory>
```

This creates: SKILL.md template with frontmatter, plus `scripts/`, `references/`, and `assets/` directories with example files. Customize or remove generated files as needed.

### Step 4: Edit the Skill

The skill is written for another Claude instance. Include only non-obvious procedural knowledge, domain details, and resource references.

#### Implement bundled resources first

Create the `scripts/`, `references/`, and `assets/` files identified in Step 2. Request user input when needed (e.g., brand assets, API docs). Delete unused example files from initialization.

#### Write SKILL.md

Writing conventions:
- Use **imperative/infinitive form** ("To accomplish X, do Y"), not second person
- Keep SKILL.md under 5k words; move detailed reference material to `references/`
- Reference every bundled resource so Claude knows when and how to use it
- Avoid duplicating content between SKILL.md and reference files

Answer these three questions in the body:
1. What is the skill's purpose? (2-3 sentences)
2. When should it trigger?
3. How should Claude use it, step by step?

### Step 5: Package the Skill

Validate and bundle the skill into a distributable zip:

```bash
scripts/package_skill.py <path/to/skill-folder>
scripts/package_skill.py <path/to/skill-folder> ./dist  # optional output dir
```

The script validates frontmatter, naming conventions, directory structure, and description quality, then packages on success. Fix any reported errors and re-run.

### Step 6: Iterate

After real-world usage, refine the skill:

1. Use the skill on real tasks
2. Note struggles or inefficiencies
3. Update SKILL.md or bundled resources
4. Re-package and test again
