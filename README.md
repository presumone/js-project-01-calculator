# Sum — Smart Calculator

**Project:** PS-JS-P01 · **Difficulty:** Beginner

A responsive browser calculator built with plain HTML, CSS, and JavaScript. Includes arithmetic, decimals, expression and result displays, clear, delete, negative numbers, keyboard support, and a session history of the latest 20 calculations.

## Run locally

Open `index.html` directly in a browser, or run `python -m http.server 8000` from this folder and open `http://localhost:8000`. No build or dependencies are required. Google Fonts is optional; system fonts work offline.

## Files

- `index.html` — semantic page and calculator controls.
- `css/style.css` — responsive layout, color palette, focus and interaction feedback.
- `js/app.js` — pure arithmetic functions, input state, events, DOM updates and history.
- `tests/calculator.test.js` — calculation and input regression tests.

## Controls

| Action | Keyboard |
| --- | --- |
| Numbers and decimals | `0–9`, `.` |
| Arithmetic | `+`, `-`, `*`, `/` |
| Calculate | `Enter` or `=` |
| Clear calculator | `Escape` or `Delete` |
| Delete character | `Backspace` |
| Navigate / activate a button | `Tab` / `Space` |

Use `+/−` to change the last operand's sign. Click a history result to reuse it. The history reset button clears history separately; history is not stored after reloading.

Multiplication and division take precedence over addition and subtraction. Operators with equal precedence evaluate left to right. After calculating, a number starts a new expression and an operator continues from the result. Consecutive operator input is ignored; use `+/−` for a negative operand. A leading minus is supported. Empty equals is a safe no-op; incomplete expressions show a useful message.

## How it works / project defense

1. **Choosing operations:** `applyOperation(left, operator, right)` selects an arithmetic operation with a `switch` and returns its value. `evaluateExpression(expression)` validates and parses the expression, reduces multiplication/division, and then addition/subtraction.
2. **Separate functions:** Arithmetic functions do not depend on the DOM, so they can be reused and tested independently. `handleInput(state, input)` owns input rules. Rendering functions handle presentation.
3. **Invalid input:** Only supported characters are accepted, each operand can have one decimal, and an operator needs a preceding number. Full validation runs before calculation. Neither `eval` nor the `Function` constructor is used. Non-finite results are rejected. Inputs are limited to 80 expression characters and 15 operand characters.
4. **Button clicks:** One delegated event listener on `.keypad` reads `data-value` or `data-action` and calls the shared `dispatch` function. DOM selection uses `querySelector`, and display updates use `textContent`.
5. **Keyboard input:** A `keydown` listener maps supported keys to the same `dispatch` function, preventing default behavior only for handled keys. Enter always calculates, and Space activates the focused button.
6. **Division by zero:** `applyOperation` throws a descriptive error. `handleInput` catches it and displays a status message, leaving the expression editable.

JavaScript uses floating-point arithmetic. Results are rounded to 12 significant digits for a readable everyday display (for example, `0.1 + 0.2` displays `0.3`). This is not an arbitrary-precision calculator. Parentheses and scientific functions are outside this project's scope; scientific notation is supported internally for very large or small results.

## Verify

Run `node --test tests/calculator.test.js` (Node.js 18+). Tests cover precedence, decimal arithmetic, invalid expressions, zero division, operator protection, deletion, reset, signs, chaining, error recovery, and input limits.

Manual checks: click and type `12.5 * 2`, confirm `25`; calculate `1 / 0`, confirm the message; delete `0`, enter `2`, confirm `0.5`; check Tab focus, history recall/reset, and a narrow mobile viewport.

## Deployment

This is a buildless static site. Sites identity is recorded in `.openai/hosting.json`. New Sites deployments are private by default. To use another static host, publish `index.html`, `css/`, and `js/` with no build command.

Deployment status: registered but not published. The installed Sites plugin's required scripts/site-workflow.mjs was unavailable during setup. Resume publishing with the existing project identity once that plugin is restored; do not register a second Site.
