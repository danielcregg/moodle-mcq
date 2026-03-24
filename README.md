# Moodle MCQ Generator

A Claude Code skill that generates Moodle quiz questions in GIFT or XML format. Built for lecturers and educators who want to rapidly create well-structured quiz questions with proper feedback, categories, and difficulty balancing.

## Installation

### Recommended (clone directly into Claude Code skills directory)

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/danielcregg/moodle-mcq.git ~/.claude/skills/moodle-mcq
```

### Manual install

```bash
mkdir -p ~/.claude/skills/moodle-mcq
cp SKILL.md ~/.claude/skills/moodle-mcq/
```

## Usage

In Claude Code, invoke the skill:

```
/moodle-mcq

Generate 20 MCQ questions on Python lists and dictionaries for first-year students
```

Or ask Claude directly:

```
Create a Moodle quiz with 10 Java OOP questions covering inheritance and polymorphism, medium difficulty
```

### Specify format

```
/moodle-mcq

Generate 15 questions on SQL joins in Moodle XML format with tags
```

## Supported Question Types

| Type | GIFT | XML | Example |
|------|------|-----|---------|
| Multiple choice (single answer) | Yes | Yes | "What is the output of...?" |
| Multiple choice (multiple answers) | Yes | Yes | "Select all that apply..." |
| True/False | Yes | Yes | "Python is a compiled language." |
| Short answer | Yes | Yes | "What keyword declares a variable in Java?" |
| Numerical | Yes | Yes | "What is 2^10?" |
| Matching | Yes | Yes | "Match the term to its definition" |
| Cloze / Fill-in-the-blank | Yes | Yes | "The ___ keyword creates a loop" |
| Calculated | No | Yes | "What is {a} + {b}?" with random values |
| Drag and drop | No | Yes | Drag items onto an image |
| Essay | Partial | Yes | Open-ended response |

## Why GIFT Format?

GIFT is the default because it uses **~6x fewer tokens** than Moodle XML for the same questions:

| Metric (10-question quiz) | GIFT | Moodle XML |
|---------------------------|------|------------|
| Approximate tokens | ~550 | ~3,500 |
| Lines of text | ~70 | ~450 |

This means you get **6x more questions per dollar** of AI usage. GIFT covers ~90% of typical quiz needs (MCQ, T/F, short answer, matching, numerical).

**Use XML when you need**: embedded images, question tags, custom penalties, shuffle control, general feedback, calculated questions, or drag-and-drop.

## Features

- **Per-answer feedback** - Explains why each option is correct or incorrect
- **Categories** - Organizes questions by module/topic for Moodle's question bank
- **Difficulty balancing** - Mix of easy (30%), medium (50%), hard (20%) based on Bloom's taxonomy
- **Code-aware** - Properly escapes special characters in programming questions
- **Plausible distractors** - Wrong answers based on real misconceptions, not random noise
- **Both formats** - GIFT for efficiency, XML for full feature set

## Bloom's Taxonomy Coverage

Questions are generated across cognitive levels:

| Level | % of Quiz | Style |
|-------|-----------|-------|
| Remember | ~15% | Recall definitions, syntax |
| Understand | ~15% | Explain concepts, trace code |
| Apply | ~25% | Use knowledge in new scenarios |
| Analyze | ~25% | Find errors, compare approaches |
| Evaluate | ~10% | Judge efficiency, best practices |
| Create | ~10% | Complete code, design solutions |

## Examples

### Python MCQ (GIFT format)

```
$CATEGORY: Programming/Python/Data Types

::Python List vs Tuple::What is the key difference between a list and a tuple in Python?{
=Lists are mutable, tuples are immutable#Correct! Lists can be modified after creation while tuples cannot.
~Lists use square brackets, tuples use parentheses#Syntax differs, but the key functional difference is mutability.
~Lists can store multiple types, tuples cannot#Both can store mixed data types.
~Tuples are faster but have no other difference#Tuples are slightly faster, but immutability is the key difference.
}
```

### Java MCQ (GIFT format)

```
$CATEGORY: Programming/Java/OOP

::Java Inheritance Keyword::Which keyword is used to inherit from a parent class in Java?{
=extends#Correct! A class uses 'extends' to inherit from another class.
~implements#'implements' is used for interfaces, not class inheritance.
~inherits#There is no 'inherits' keyword in Java.
~super#'super' calls the parent constructor or methods, not for declaring inheritance.
}
```

## Importing into Moodle

### GIFT files
1. Go to your Moodle course
2. Navigate to **Question bank** > **Import**
3. Select **GIFT format**
4. Upload your `.gift` file
5. Click **Import**

### XML files
1. Same steps, select **Moodle XML format** at step 3

## Requirements

- [Claude Code](https://claude.ai/download) CLI
- A Moodle LMS instance (for importing generated questions)

## License

MIT
