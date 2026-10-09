# Copilot instructions

## Build and verification

- Preview the app by opening `index.html` directly in a browser. It is self-contained and has no build step.
- There are no automated test or lint commands configured.
- Single browser smoke path: answer question 1 with **B** (correct), question 2 with **A** (incorrect), then answer the remaining questions correctly. Confirm the score changes to 1, stays at 1 after the wrong answer, feedback reveals the correct choice, and the results screen shows 9/10 with the explorer reaction. Use the replay button to check reset behavior.

## Architecture

- `index.html` contains the whole app: document markup, inline CSS, question data, state, and rendering logic.
- The `questions` array is the source of truth for the quiz. `renderQuestion()` replaces `#quiz-panel`; `chooseAnswer()` locks choices, updates score and feedback, then advances; `renderResults()` selects the score-based reaction; `restartQuiz()` resets the state.
- Question UI is regenerated on each step, so event listeners for answer buttons are attached after `renderQuestion()` writes the new panel markup.

## Code conventions

- Each question has four answers. `correct` is a zero-based index into `answers`; the A–D keyboard shortcuts and visible letter labels rely on that four-choice order.
- Keep quiz state transitions coordinated across `renderQuestion()`, `chooseAnswer()`, `renderResults()`, and `restartQuiz()`. Correct answers increase the score; incorrect answers reveal the correct choice without changing it.
- Use the CSS custom properties in `:root` and their `prefers-color-scheme: dark` overrides for theme-aware styling. Respect the existing `prefers-reduced-motion` rules when adding animation.
- Preserve native answer/replay buttons, the question heading used to label the answer group, and the live feedback/status semantics when changing rendered markup.
