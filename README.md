# Moodle MCQ

A Claude Code skill for generating, reviewing, and improving Moodle quiz questions. Supports GIFT, Moodle XML, and Aiken formats. Built for lecturers and educators who create assessments for Moodle LMS.

## Installation

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/danielcregg/moodle-mcq.git ~/.claude/skills/moodle-mcq
```

## Usage

### Create questions (standard difficulty)
```
/moodle-mcq

Generate 20 MCQ questions on Python lists and dictionaries for first-year students
```

### Create challenging questions (60-75% target success rate)
```
/moodle-mcq challenging

Generate 15 hard questions on Java OOP covering inheritance and polymorphism
```

### Review and improve existing questions
```
/moodle-mcq review

[paste your existing questions here]
```

### Specify output format
```
/moodle-mcq

Generate 10 questions on SQL joins in Moodle XML format with syntax highlighting
```

## Three Modes

| Mode | Use For | Target Success Rate |
|------|---------|-------------------|
| **Create Standard** | General quizzes, revision, formative assessment | 75-85% |
| **Create Challenging** | Exams, summative assessment, advanced students | 60-75% |
| **Review** | Improving existing MCQs — fix weak distractors, balance lengths, remove lecture references | N/A |

## Three Output Formats

| Format | Tokens per 10 Questions | Best For |
|--------|------------------------|----------|
| **GIFT** (default) | ~550 | Standard quizzes without images |
| **Moodle XML** | ~3,500 | Syntax-highlighted code, images, tags |
| **Aiken** | ~300 | Quick review output |

## Key Features

### Question Quality
- **Self-contained stems** — no references to lectures or slides, all code included in questions
- **Plausible distractors** — based on real misconceptions, not random noise
- **Answer length balance** — enforces 15/15/70 distribution (shortest/longest/middle) to prevent pattern exploitation
- **Bloom's taxonomy coverage** — questions span remember through create levels

### Challenging Mode
- 5 distractor design strategies (overcorrection, outdated practice, wrong context, incomplete solution, reasonable misunderstanding)
- Gradients of correctness — options range from completely right to subtly wrong
- Tests judgment and trade-offs, not just recall
- Scenario-based questions with real-world constraints

### Review Mode
- Difficulty assessment with success rate estimation
- Weak distractor identification (throwaway options, extreme language, length imbalance)
- 5 distractor replacement techniques
- Lecture reference removal
- Length rebalancing

### Code Questions
- Java syntax highlighting with inline CSS (blue keywords, green numbers, red strings)
- Python syntax highlighting with color scheme
- GIFT special character escaping for programming questions
- Complete code always included in question stem

### Moodle Compatibility
- Critical `<name>` tag fix (prevents import errors)
- No penalty tags (omitted entirely)
- Proper CDATA sections
- Category structure with `$course$/Category Name`
- `shuffleanswers=true`, `answernumbering=abc`

## Supported Question Types

| Type | GIFT | XML | Aiken |
|------|------|-----|-------|
| Multiple choice (single answer) | Yes | Yes | Yes |
| Multiple choice (multiple answers) | Yes | Yes | No |
| True/False | Yes | Yes | No |
| Short answer | Yes | Yes | No |
| Numerical | Yes | Yes | No |
| Matching | Yes | Yes | No |
| Cloze / Fill-in-the-blank | Yes | Yes | No |
| Calculated | No | Yes | No |
| Drag and drop | No | Yes | No |

## Review Document

Every generation includes a review document with:
- Length distribution summary table
- Per-question length annotations (SHORTEST/MIDDLE/LONGEST)
- Correct answers in bold
- Category organization
- Difficulty mode used

## Importing into Moodle

1. Go to your Moodle course
2. Navigate to **Question bank** > **Import**
3. Select your format (GIFT, Moodle XML, or Aiken)
4. Upload the file
5. Click **Import**

## Version History

- **v3.0** (2026-03-24): Major merge — combined 4 skills (moodle-mcq-creator, hard-questions, mcq-reviewer-improver, mcq-reviewer-skill) into single skill with 3 modes. Added GIFT and Aiken format support. Added Python syntax highlighting. Published to GitHub.
- **v2.x** (2025-2026): Individual skills for standard questions, hard questions, and reviewing
- **v1.x** (2025): Initial skill for AI Assisted Programming module

## Requirements

- [Claude Code](https://claude.ai/download) CLI
- A Moodle LMS instance (for importing generated questions)

## License

MIT
