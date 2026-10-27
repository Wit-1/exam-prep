# exam-prep

An exam-oriented review Skill. At its core, it is **a set of stable, reusable, score-driven review rules** rather than a chain of utility scripts.

It fits scenarios like these:

- You already have your own course materials
- You want an AI to clean your notes, extract knowledge points, and generate practice questions with explanations
- You want questions that mirror real exams, not random question-bank style
- You want the explanations to also cover "what to write to actually score points"

## What This Skill Does

`exam-prep` guides the Agent through the following workflow:

1. Confirm the exam question types, instructor's emphasis, and common test patterns
2. Convert uploaded materials to Markdown first, using `markitdown` whenever possible
3. Clean the Markdown, then extract knowledge points, then generate questions
4. Judge what is worth reviewing by source priority
5. Generate `1–6` questions per knowledge point, weighted by density and test likelihood
6. Separate questions from answers by default
7. For objective questions, append a "Core Knowledge Point" note
8. For subjective questions, append "Core Scoring Point", "Must-Include Elements", and "Common Pitfalls"
9. Make every explanation serve exam scoring

## Core Rules

### Explanation Detail

- Adapt chapter notes, answers, and explanations to the user's stated learning situation and requested level of detail.
- No background / never studied the subject: be as detailed as possible. Explain terminology, symbols, and necessary prerequisites, add simple examples where helpful, and walk through the reasoning, steps, and reasons behind each step.
- Studied but rusty / weak foundations: focus on weak areas, conceptual connections, and common confusions while retaining complete solutions.
- Already proficient / reviewing, or explicitly requesting brevity: focus on core conclusions, applicability conditions, formulas, pitfalls, standard answers, and key steps.
- Do not ask again about information already provided; use the usual level of detail when unspecified. These are examples rather than fixed levels; adjust to later requests. Keep exam-style question wording and answer separation; the final one-page review sheet remains concise.

### Question Source Priority

When generating questions and judging what to emphasize, rank only the materials the user has provided; skip missing categories without requiring a complete set:

1. Topics explicitly emphasized by the instructor in class
2. Past exam papers
3. Regular homework / reading assignments
4. Instructor's slides
5. Crash-course questions

The instructor's emphasis may come from the user's account or annotations in the materials; do not treat ordinary slide content as explicitly emphasized. AI-supplemented questions are outside this material ranking: use them only when materials are insufficient and label them clearly.

Notes:

- Crash-course questions rank lower in priority
- However, crash courses are highly valuable for organizing knowledge points, building chapter structure, and summarizing key points

### Material Processing Rules

All materials should be converted to Markdown first. The recommended tool is:

```bash
markitdown
```

The unified workflow is:

1. Convert to Markdown
2. Clean the Markdown
3. Extract knowledge points
4. Generate questions last

Cleaning priorities:

- Remove garbled characters
- Remove duplicates
- Remove filler content
- Pull out core definitions, properties, conclusions, methods, and common pitfalls

The goal is not "complete archival" — it is "clear, memorable, and exam-ready".

### Question Generation Rules

- Generate questions by real exam format, not by mechanical distribution
- Typically `1–6` questions per knowledge point
- Denser, higher-frequency, and more testable knowledge points get more questions
- Question type must match the most likely exam style for the knowledge point

### Question Source Tagging Rules

Strict file-level or page-level traceability is not required by default.

However, keep lightweight category-level tags, for example:

- `Source: Instructor's in-class emphasis`
- `Source: Past exam papers`
- `Source: Instructor slides`
- `Source: Regular homework`
- `Source: Crash-course framework adapted`
- `Source: AI supplemented`

### Output Rules

Plain-text output:

- Only questions appear at the top
- All answers and explanations are grouped at the end
- The question section and answer section must be clearly separated

HTML output:

- Answers are placed under the question
- Answers are collapsed by default
- They expand on click to reveal the full answer and explanation

### Explanation Rules

Explanations should serve scoring, not merely explain correctness.

For objective questions, emphasize:

- `Core knowledge point of this question`
- Key definitions, properties, and judgment criteria
- Commonly confused points

For subjective questions, emphasize:

- `Core scoring point of this question`
- `Must-include elements`
- `Common pitfall`
- A standard answer structure

Whenever possible, explanations should also cover:

- Which words must appear in the answer
- What to write first to grab step-by-step points
- Which mistakes most easily cost you marks

### Problem Recording Rules

- When setting up a subject, create `{subject folder}/错题集.md` with only a course title; reuse an existing file without overwriting its records.
- Append a record only when the user explicitly asks to record it. Wrong or uncertain answers and follow-up questions alone do not trigger recording.
- Briefly capture the problem, the user's stated confusion, and the explanation from the actual conversation. No fixed template, classification, statistics, or resolution status is required; do not invent the user's understanding.
- If the user asks to record the same problem again, append the new content and preserve earlier records.
- Existing records may guide exam review; having no records does not block the final one-page review sheet or other review steps.

## Repository Structure

This repository is centered on a **rule-based Skill**:

- [`SKILL.md`](./SKILL.md) — the full workflow and rules
- [`AGENT.md`](./AGENT.md) — a long-form default ruleset you can copy directly into an Agent / Claude configuration

## How to Use

Invoke `$exam-prep` in an Agent that supports skill calls, then follow these three steps:

1. **Give the Agent as many of your available materials as possible**, such as topics explicitly emphasized by the instructor in class, past exam papers, regular homework / reading assignments, instructor slides, and crash-course questions. Materials are prioritized in that order; missing categories do not prevent getting started.
2. **Tell the Agent about your current learning situation**, for example: "I have no background and need concepts and symbols explained from scratch," "I have studied the course once but am rusty on formulas and methods," or "I know the basics and only need key points and common pitfalls." You can also identify weak chapters and specify how detailed the explanations should be.
3. **Ask the Agent to start organizing the materials**. Based on your materials and learning situation, it will clean the content, organize notes, and generate questions with answers and explanations suited to your background.

If your Agent supports long-term rule files, you can sync the rules from [`AGENT.md`](./AGENT.md) directly into your own `AGENT.md` or `CLAUDE.md`.

In addition, one of the design goals of this skill is to **automatically check and sync such long-term rule files** after running, so that:

- The complete workflow in `SKILL.md`
- The long-term default rules in `AGENT.md` / `CLAUDE.md`

stay in sync and do not drift into two conflicting logics.

## Subject Extensions

Contributions of subject-specific content are highly welcome, for example:

- Calculus
- Discrete Mathematics
- Data Structures
- Computer Networks
- Operating Systems

If you have unique study techniques, question-type heuristics, answer templates, or already-cleaned Markdown notes for a specific subject, feel free to extend this repository on top of it.

Suggested directions:

- Subject-specific question type summaries
- High-frequency test points
- Common pitfalls
- Answer templates for proof, computation, and short-answer questions
- Subject-specific Markdown material
- Subject-specific Skill rules

When developing, please organize your work in a feature branch and submit a Pull Request to merge it back, rather than scattering content across long-lived branches.

## Contributing

Contributions that improve this Skill from real review experience are welcome. Especially valuable:

- Better question source priority strategies
- More robust Markdown cleaning rules
- Question generation protocols that match real exams more closely
- Stronger objective / subjective question explanation templates
- More practical exam-scoring technique summaries
- Subject-specific review rules and materials for a concrete course

Recommended contribution flow:

1. Fork this repository
2. Create a new branch from your fork
3. Modify `SKILL.md`, `AGENT.md`, `README.md`, or add subject-specific material on the branch
4. Commit your changes
5. Push to your fork
6. Open a Pull Request to the main repository
7. In the PR, describe:

   - What you changed
   - Why you changed it
   - Which review scenario it applies to

If your contribution is tailored to a personal course, please mark the applicable scope in the PR so reviewers can decide whether to merge it into the main branch.
