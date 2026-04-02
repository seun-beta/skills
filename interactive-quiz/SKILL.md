---
name: interactive-quiz
description: Build interactive multiple-choice quizzes rendered as HTML artifacts with per-option personalized feedback, progress tracking, and score breakdowns. Use this skill whenever the user wants to create a quiz, test their knowledge, study for an exam, make flashcards, generate practice questions, review chapter objectives, or asks to be quizzed on any topic. Also trigger when the user provides learning objectives, chapter summaries, or study material and wants to test themselves. Even casual requests like "quiz me", "test me on this", "give me some practice questions", or "I want to study for my exam" should trigger this skill.
---

# Interactive Quiz Builder

Build self-contained interactive HTML quiz artifacts with rich per-option feedback, multi-select support, progress tracking, and score breakdowns by category.

## When to use

- User wants to be quizzed or tested on any topic
- User provides learning objectives or chapter content and wants practice questions
- User asks for multiple choice questions, multi-select questions, or mixed formats
- User wants study/review tools for exams or certifications

## Quiz Design Principles

### Question Quality
- Write questions that test understanding, not just recall
- Each wrong answer (distractor) should represent a plausible misconception
- For multi-select questions, choose topics where there are naturally multiple correct items (e.g., "which of these are display managers?")
- Mix difficulty levels across the quiz
- Tag each question with a category that maps to the user's learning objectives

### Feedback Quality — The Core Differentiator
Every single option in every question gets its own tailored explanation. This is what makes this quiz tool valuable — not generic "correct/incorrect" messages, but specific feedback that addresses the user's exact misconception.

**For wrong answers:** Explain what the option actually is, why someone might confuse it for the correct answer, and why it doesn't fit this question. Example: "Nautilus is GNOME's file manager for browsing files — it has nothing to do with login screens. A helpful hint: display managers typically end in 'DM'."

**For correct answers:** Reinforce why it's correct with additional context. Example: "Exactly right! GDM (GNOME Display Manager) is the default login manager for GNOME-based distributions like Ubuntu and Fedora."

**For multi-select missed answers:** Explain why the user should have picked this one. Example: "You missed this one — SDDM is the default display manager for KDE Plasma desktops."

### Feedback Display Rules

**Single choice questions:**
- If correct: Green box with explanation of why it's right
- If wrong: Red box explaining why the picked answer is wrong + Green box explaining why the correct answer is right

**Multi-select questions:**
- Wrong picks: Red box per wrong pick with explanation
- Missed correct answers: Gold box (background: #FAC775, border: #BA7517) per missed answer with "Missed" label on the button and explanation of why it should have been selected
- Correctly selected: Green acknowledgment box listing what they got right
- All correct: Single green celebration box

## Building the Quiz

### Step 1: Gather Requirements
Understand what the user wants to be tested on. They may provide:
- Learning objectives from a textbook chapter
- A topic area (e.g., "Linux filesystems")
- Specific subtopics or concepts
- Preferred question count (default to 10-12)
- Whether they want single-choice, multi-select, or mixed (default to mixed)

If the user says "I'll tell you when I'm ready," acknowledge and wait. Don't generate the quiz until they say go.

### Step 2: Generate Questions and Feedback
For each question, create:
- `q`: The question text
- `opts`: Array of 4 option strings
- `ans`: Array of correct answer indices (single element for single-choice, multiple for multi-select)
- `multi`: Boolean flag
- `cat`: Category string matching a learning objective
- `fb`: Object with keys "0", "1", "2", "3" — each containing a tailored 1-2 sentence explanation for that specific option

Aim for 10-12 questions with good coverage across all provided learning objectives.

### Step 3: Build the Artifact
Generate the quiz as a single self-contained HTML artifact using the show_widget visualizer tool. The artifact must include:

1. **Progress bar** — dots showing completion
2. **Category tags** — color-coded per learning objective
3. **Question type indicator** — "Single answer" or "Select all that apply"
4. **Clickable options** — radio-style for single, checkbox-style for multi-select
5. **Submit button** — for multi-select only (appears after first selection)
6. **Rich feedback boxes** — appearing after answer, color-coded per result type
7. **Next button** — appears after answering
8. **Results screen** — final score, progress bar, breakdown by category, retry button

### Critical Implementation Details

Reference the template in `references/quiz-template.md` for the full HTML/CSS/JS implementation. Key points:

- All feedback is pre-embedded as a `fb` object on each question — no API calls needed
- Multi-select requires clicking a "Submit answers" button before scoring
- After submit, remove the `.sel` class from buttons before applying result classes
- Gold highlight for missed answers uses `!important` to override: `border-color: #BA7517 !important; background: #FAC775 !important; color: #412402 !important;`
- Append a "Missed" label span to missed answer buttons
- Category colors should be distinct and use the visualizer color palette (purple, teal, coral, blue, etc.)
- The quiz container should have `max-width: 600px`
- Use CSS variables for theme compatibility (backgrounds, borders, text colors)

### Color Assignments for Categories
Assign category colors from this set, cycling as needed:
- Category 1: `#534AB7` (purple)
- Category 2: `#1D9E75` (teal)
- Category 3: `#D85A30` (coral)
- Category 4: `#378ADD` (blue)
- Category 5: `#D4537E` (pink)

### Results Screen
Show:
- Large score fraction (e.g., "8/12")
- Encouraging message scaled to performance (100%, 80%+, 60%+, below 60%)
- Animated progress bar fill
- Breakdown table showing score per category with colored dots
- "Try again" button that resets all state

## Template Reference

Before building the quiz artifact, read the full template at `references/quiz-template.md` for the complete HTML/CSS/JS implementation that handles all the above requirements. Copy and adapt it, inserting the generated questions and feedback data.
