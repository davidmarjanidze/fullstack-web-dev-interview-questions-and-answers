---
name: interview-qa
description: Create a markdown file of 450 interview questions and answers (150 easy, 150 moderate, 150 advanced) for a specific technology, framework, language, or tool. Use whenever the user asks to add, create, or generate interview questions and answers for a technology, e.g. "add interview questions and answers for DynamoDB".
---

# Interview Questions and Answers Generator

Create one markdown file containing 450 interview questions and answers for the requested technology: 150 easy, 150 moderate, 150 advanced.

## Step 1: Pick the file location and name

- Place the file in the folder that best matches the technology's category. Existing folders: `front-end/`, `back-end/`, `ai-engineering/`, `optimization/`, `mix/`. Use `mix/` only if nothing else fits. Create a new folder only if the user asks.
- Name the file in lowercase kebab-case: `dynamodb.md`, `react-unit-testing.md`, `redux-toolkit.md`.
- Before writing, check whether the file already exists. If it does, ask the user whether to overwrite it or stop.
- Look at one existing file in the repo (for example `front-end/angular.md`) and match its tone and formatting.

## Step 2: Verify the current state of the technology

Do not rely on memory alone.

- Use web search or the official documentation to find the latest stable version and its current APIs, features, and best practices.
- Note what is deprecated or removed. Teach the modern approach first. Mention legacy approaches only where candidates will still meet them in real projects, and label them as legacy.
- Do not invent APIs, flags, or method names. If unsure that something exists, check it or leave it out.

## Step 3: File structure

The file must follow this exact structure:

1. `# <Technology> Interview Questions and Answers` as the H1 title.
2. A short intro paragraph (2-4 sentences) stating the question count, the difficulty split, and which version of the technology the answers target.
3. `## Table of Contents` with links to the three sections:
   - `[Easy Questions (1-150)](#easy-questions-1-150)`
   - `[Moderate Questions (151-300)](#moderate-questions-151-300)`
   - `[Advanced Questions (301-450)](#advanced-questions-301-450)`
4. Three main sections, in this order, each with a `###` header:
   - `### Easy Questions (1-150)`
   - `### Moderate Questions (151-300)`
   - `### Advanced Questions (301-450)`

## Step 4: Question format

Number questions continuously from 1 to 450 across the whole file. Do not restart numbering per section. Use this format:

```
**1: What is <concept>?**

Answer text.

**2: How do you <task>?**

Answer text, with a code example if useful.
```

## Step 5: Content rules

- **Easy (1-150):** fundamentals, core concepts, terminology, basic syntax, and common everyday usage.
- **Moderate (151-300):** practical usage, common patterns, configuration, integration with other tools, debugging, and trade-offs.
- **Advanced (301-450):** internals, architecture, performance tuning, scalability, security, edge cases, design decisions, and scenario-based questions.
- Add a code example wherever it helps the answer, in fenced code blocks with the correct language tag. Code must be correct, runnable, and use current APIs.
- Keep answers concise but complete: 2-6 sentences for conceptual answers, plus code where needed.
- No duplicate or near-duplicate questions. Each question must cover a distinct topic.
- Cover the technology broadly: every major feature area should appear at least once.
- Phrase questions the way real interviewers ask them.

## Step 6: Writing process

450 Q&As is too large for a single write. Work in batches:

1. Create the file with the title, intro, table of contents, and the three section headers.
2. Append the questions in batches of about 25-50, in numerical order, so the numbering stays continuous.
3. After finishing, verify the file: exactly 450 questions, numbered 1-450 with no gaps or duplicates, 150 per section, table of contents links matching the headers, and all code blocks closed properly.
4. Report the file path and a one-line summary when done.

## Step 7: Register the technology in README.md

`README.md` lists every technology covered by this repo. After the Q&A file is complete, add the new technology to that list.

- Follow the existing format exactly: same bullet or table style, same link style, same capitalization pattern, same grouping.
- If the README groups technologies by category (front-end, back-end, and so on), add the entry under the category that matches the folder the file was placed in. If a matching group doesn't exist, create one in the same style as the others.
- Keep the existing ordering convention. If the list is alphabetical, insert the entry alphabetically. If it is not sorted, append it at the end of its group.
- The link must point to the new file's relative path, for example `back-end/dynamodb.md`.
- Do not add a duplicate. If the technology is already listed, update that entry's link instead of adding a second one.
- Do not change any other part of the README.

## Step 8: Final check and report

- Confirm the README entry exists, appears exactly once, and its link path matches the actual file path.
- Report back with the Q&A file path, the README line that was added or changed, and a one-line summary.
