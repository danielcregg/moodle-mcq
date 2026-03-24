<h1 align="center">Moodle MCQ</h1>

<p align="center">
  <strong>A Claude Code skill for generating, reviewing, and improving Moodle quiz questions</strong>
</p>

<p align="center">
  <a href="https://github.com/danielcregg/claude-code-skill-moodle-mcq/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT"></a>
  <img src="https://img.shields.io/badge/Version-3.0.0-green.svg" alt="Version 3.0.0">
  <img src="https://img.shields.io/badge/Platform-Claude%20Code-blueviolet.svg" alt="Platform: Claude Code">
  <img src="https://img.shields.io/badge/Moodle-4.x%20Compatible-orange.svg" alt="Moodle 4.x Compatible">
  <img src="https://img.shields.io/badge/Formats-GIFT%20%7C%20XML%20%7C%20Aiken-informational.svg" alt="Formats: GIFT | XML | Aiken">
</p>

<p align="center">
  Create exam-ready Moodle quizzes directly from Claude Code. Supports GIFT (~6x fewer tokens than XML), Moodle XML (with syntax highlighting), and Aiken formats. Enforces answer-length balancing, self-contained stems, and plausible distractors based on real pedagogical best practices.
</p>

---

## Quick Start

```bash
# Install
mkdir -p ~/.claude/skills
git clone https://github.com/danielcregg/claude-code-skill-moodle-mcq.git ~/.claude/skills/moodle-mcq

# Use in Claude Code
/moodle-mcq
Generate 20 MCQ questions on Python data structures for first-year students
```

---

## Three Modes

| Mode | Invoke With | What It Does | Target Success Rate |
|------|------------|--------------|-------------------|
| **Create Standard** | `/moodle-mcq` | Generate conceptual questions with plausible distractors | 75-85% |
| **Create Challenging** | `/moodle-mcq challenging` | Exam-grade questions testing judgment and trade-offs | 60-75% |
| **Review & Improve** | `/moodle-mcq review` | Assess existing MCQs, fix weak distractors, rebalance lengths | N/A |

---

## Three Output Formats

| Format | Tokens per 10 Qs | Best For | Moodle Import Path |
|--------|------------------|----------|-------------------|
| **GIFT** (default) | ~550 | Standard quizzes, fastest generation | Question bank > Import > GIFT |
| **Moodle XML** | ~3,500 | Code with syntax highlighting, images, tags | Question bank > Import > Moodle XML |
| **Aiken** | ~300 | Quick review, simplest format | Question bank > Import > Aiken |

> GIFT uses approximately **6x fewer tokens** than Moodle XML for the same questions. For a 50-question quiz, that is ~2,750 tokens (GIFT) vs ~17,500 tokens (XML).

---

## Before & After

### Create Mode: Standard vs Challenging

<table>
<tr>
<th width="50%">Standard (75-85% success rate)</th>
<th width="50%">Challenging (60-75% success rate)</th>
</tr>
<tr>
<td>

```
What keyword declares inheritance in Java?

A) extends          <-- correct
B) implements
C) inherits
D) super
```

Tests: single-fact recall

</td>
<td>

```
Your AI assistant generates a database query
using string concatenation. It passes all tests.
Which issue has highest priority?

A) SQL injection from concatenation  <-- correct
B) SELECT * exposes unnecessary PII
C) Password needs timing-safe comparison
D) Missing rate limiting enables brute force
```

Tests: prioritization across **4 real security concerns**

</td>
</tr>
</table>

### Review Mode: Before & After

<table>
<tr>
<th width="50%">Before (too easy, 90%+ success rate)</th>
<th width="50%">After (appropriate, ~60% success rate)</th>
</tr>
<tr>
<td>

```
According to the lecture, what causes a workflow
to begin in GitHub Actions?

A) An event trigger defined in the YAML file
B) A manual button click each time
C) Scheduled cron job every hour
D) Administrator approval
```

Problems:
- References lecture
- B, C, D are obviously wrong
- Length imbalance

</td>
<td>

```
A developer wants their CI workflow to run when
code is pushed. What causes a GitHub Actions
workflow to begin execution?

A) An event trigger defined in the workflow YAML
   that responds to push or pull request events
B) A webhook configuration mapping Git operations
   to corresponding workflow files
C) A GitHub App that monitors repository events
   and dispatches workflows through the Actions API
D) A branch protection rule specifying which
   workflows must pass before merging
```

Fixes: no lecture ref, all plausible, balanced lengths

</td>
</tr>
</table>

---

## Supported Question Types

| Type | GIFT | XML | Aiken |
|------|:----:|:---:|:-----:|
| Multiple choice (single answer) | Yes | Yes | Yes |
| Multiple choice (multi-answer) | Yes | Yes | - |
| True / False | Yes | Yes | - |
| Short answer | Yes | Yes | - |
| Numerical | Yes | Yes | - |
| Matching | Yes | Yes | - |
| Cloze / Fill-in-the-blank | Yes | Yes | - |
| Calculated | - | Yes | - |
| Drag and drop | - | Yes | - |

---

## Key Features

### Answer Length Balancing (15/15/70 Rule)

Students exploit patterns when correct answers are systematically longer. This skill enforces a mandatory distribution and includes a verification table in every review document:

| Correct Answer Position | Target | Purpose |
|------------------------|--------|---------|
| **Shortest** option | ~15% | Prevents "longest = correct" bias |
| **Longest** option | ~15% | Prevents "shortest = correct" bias |
| **Middle** length | ~70% | No exploitable pattern |

<details>
<summary><strong>How it works</strong></summary>

1. After drafting all questions, the skill computes each option's character count (excluding HTML markup)
2. Classifies each correct answer as Shortest / Longest / Middle
3. Tallies across all questions to verify the 15/15/70 target
4. Rebalances if needed by adjusting distractor verbosity or trimming correct answers
5. Includes the distribution table in the review document for instructor verification

</details>

### Self-Contained Stems

Every question must stand alone without external references:

| Rule | Bad Example | Good Example |
|------|------------|--------------|
| No lecture refs | "According to the lecture..." | "In Python..." |
| No slide refs | "As shown on slide 12..." | Context in the stem |
| No example refs | "In the Person class example..." | Include the actual code |
| Code included | "What does the setter do?" | Show the full code in the question |

### Five Distractor Strategies (Challenging Mode)

<details>
<summary><strong>Click to expand</strong></summary>

| Strategy | Description | Example |
|----------|------------|---------|
| **Overcorrection** | Takes a good practice too far | "Review every line with the entire team" |
| **Outdated Practice** | Was correct in older versions | "Use class components for all React code" |
| **Wrong Context** | Correct for different situations | "Use SELECT * for better performance" |
| **Incomplete Solution** | Addresses part of the problem | "Add unit tests" (when security review also needed) |
| **Reasonable Misunderstanding** | Based on incomplete knowledge | "Copilot adapts based on package.json versions" |

Each distractor is tested against: *Can an expert eliminate this without thinking? Would a B-grade student find it tempting? Does it represent a real misconception?*

</details>

### Code Syntax Highlighting (XML Mode)

When generating Moodle XML, code questions include inline CSS syntax highlighting that renders correctly in Moodle without any plugins:

**Java**: blue keywords, green numbers, red strings, green comments
**Python**: blue keywords, teal built-ins, green numbers, red strings

<details>
<summary><strong>Example: Java code in Moodle XML</strong></summary>

```xml
<questiontext format="html">
  <text><![CDATA[<p>What happens when a username longer than 10 characters is passed?</p>
<pre style="background-color: #f4f4f4; padding: 10px; border-radius: 5px;
  border-left: 3px solid #4CAF50; font-family: 'Courier New', monospace;">
<span style="color: #0000ff;">public void</span> setUsername(String username) {
    <span style="color: #0000ff;">if</span> (username.length() > <span style="color: #098658;">10</span>) {
        <span style="color: #0000ff;">this</span>.username = username.substring(<span style="color: #098658;">0</span>, <span style="color: #098658;">10</span>);
    }
}</pre>]]></text>
</questiontext>
```

</details>

### Moodle XML Compatibility

Built-in fixes for common Moodle import issues:

| Issue | Fix |
|-------|-----|
| `<n>` instead of `<name>` | Enforces full `<name>` tag (prevents "Missing question name" error) |
| Penalty tags | Omitted entirely (no penalty for wrong answers) |
| Generic titles | Requires descriptive names (e.g., "Array_Index_Exception") |
| Missing CDATA | All text wrapped in `<![CDATA[...]]>` sections |
| Category format | Uses `$course$/Category Name` structure |

---

## Review Document

Every generation includes a human-readable review document (`_review.md`) for instructor verification:

```markdown
# Python Data Structures - Quiz Review

## Difficulty Mode: Standard
## Output Format: GIFT

## Length Distribution Summary
| Correct Answer Position | Count | Percentage | Target |
|------------------------|-------|------------|--------|
| Shortest               | 3     | 15%        | ~15%   |
| Longest                | 3     | 15%        | ~15%   |
| Middle                 | 14    | 70%        | ~70%   |

## Category: Lists and Tuples

### Question 1: List_vs_Tuple_Mutability
*Correct answer length: MIDDLE*

What is the key difference between a list and a tuple in Python?

A) Tuples are faster but have no other difference
B) **Lists are mutable, tuples are immutable**
C) Lists can store multiple types, tuples cannot
D) Lists use square brackets, tuples use parentheses
```

---

## GIFT Format Example

```
$CATEGORY: Programming/Python/Data Types

::List vs Tuple::What is the key difference between
a list and a tuple in Python?{
=Lists are mutable, tuples are immutable#Correct!
  Lists can be modified after creation while tuples cannot.
~Lists use square brackets, tuples use parentheses#Syntax
  differs, but the key functional difference is mutability.
~Lists can store multiple types, tuples cannot#Both
  can store mixed data types.
~Tuples are faster but have no other difference#Tuples
  are slightly faster, but immutability is the key difference.
}
```

> **Note for programming questions**: GIFT requires escaping `~ = { } # :` characters. For code-heavy questions, Moodle XML with `<![CDATA[<pre>...</pre>]]>` is recommended to avoid escaping issues.

---

## File Structure

```
~/.claude/skills/moodle-mcq/
  SKILL.md        # Skill definition (the brain)
  README.md       # This file
  LICENSE          # MIT
```

---

## Compatibility

| Platform | Supported |
|----------|:---------:|
| Claude Code (CLI) | Yes |
| Moodle 4.x | Yes |
| Moodle 3.x | Yes |
| Windows / macOS / Linux | Yes |

---

## Installation

### Option 1: Git Clone (recommended)

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/danielcregg/claude-code-skill-moodle-mcq.git ~/.claude/skills/moodle-mcq
```

### Option 2: Manual Download

Download `SKILL.md` from this repo and place it at:
```
~/.claude/skills/moodle-mcq/SKILL.md
```

### Verify Installation

In Claude Code, type `/moodle-mcq` — if the skill loads, you are good to go.

---

## Usage Examples

### Generate a standard quiz from lecture slides
```
/moodle-mcq

I have a lecture on Java OOP (inheritance, polymorphism, encapsulation).
Generate 15 MCQ questions in GIFT format, 5 per topic.
```

### Generate a challenging exam
```
/moodle-mcq challenging

Create 20 hard questions on Python error handling and exceptions.
Use Moodle XML format with syntax-highlighted code snippets.
Target 60-70% success rate.
```

### Review and improve existing questions
```
/moodle-mcq review

[paste existing questions in any format]

Fix weak distractors, balance answer lengths, remove any lecture references.
```

### Generate from PowerPoint slides
```
/moodle-mcq

Extract content from lecture.pptx and generate 25 questions across all topics.
```

The skill uses `python -m markitdown lecture.pptx` to extract slide content when PowerPoint files are provided.

---

## Importing into Moodle

1. Log into Moodle as a teacher or admin
2. Navigate to your course
3. Go to **Question bank** > **Import**
4. Select the format matching your file:
   - `.gift` files: select **GIFT format**
   - `.xml` files: select **Moodle XML format**
   - `.txt` files: select **Aiken format**
5. Upload the file and click **Import**
6. Review the imported questions in the question bank

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| **3.0.0** | 2026-03-24 | Major merge: combined 4 skills into one. Added GIFT + Aiken formats. Added Python syntax highlighting. Published to GitHub. |
| 2.3 | 2026-02-12 | Merged reviewer skills into single `mcq-reviewer` |
| 2.2 | 2026-02-12 | Merged creator skills with difficulty mode parameter |
| 2.1 | 2025-12-03 | XML tag docs fix, length distribution tables |
| 2.0 | 2025-11-06 | Difficulty calibration overhaul, 5 distractor strategies |
| 1.4 | 2025-10-30 | Java syntax highlighting CSS template |
| 1.3 | 2025-10-30 | Self-contained code requirement |
| 1.2 | 2025-10-30 | Critical `<name>` tag fix for Moodle import |
| 1.0 | 2025-10-23 | Initial skill for AI Assisted Programming module |

---

## Contributing

Contributions are welcome. If you have ideas for improving question generation quality, supporting additional Moodle question types, or fixing edge cases:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## License

[MIT](LICENSE) - use it however you like.
