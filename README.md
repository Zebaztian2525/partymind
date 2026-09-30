# Partymind

A browser-based code-breaking puzzle where the code is a Swedish coalition.
Four seats, eight parties, ten guesses. Inspired by the classic board game
*Mastermind*, built from scratch as a single-file HTML/CSS/JavaScript app.

**▶ [Play it here](https://Zebaztian2525.github.io/partymind/)**

---

## How to play

The computer has picked a secret sequence of four parties (duplicates
allowed). Your job is to figure out the coalition before you run out of
tries.

1. **Drag** a party from the palette onto any seat in the current row, or
   **click** a party and then click a seat.
2. Click a filled seat to clear it, or use **Undo** to remove the last party.
3. Press **Guess** when all four seats are filled.

After each guess you get feedback:

| Marker | Meaning |
|--------|---------|
| ⚫ Black | Correct party in the correct seat |
| ⚪ White | Correct party, but in the wrong seat |

Markers are not tied to specific seats — they only tell you *how many* of
each.

You have **10 guesses**. A small comment appears next to each submitted row,
noting what kind of government your four parties would form.

---

## The challenge

On the surface, Partymind works exactly like the classic code-breaking
game: four positions, eight symbols, ten guesses, black and white feedback.
Learn the feedback rules once and you can play any version of it.

But there is a second layer, and it is the one that trips you up.

The symbols are real. V is not just "red tile number one" — it is
Vänsterpartiet. M is Moderaterna. And the moment you start placing them next
to each other, your political intuition wakes up and starts arguing with the
game logic. *S and SD together? Unthinkable. M and MP? Wrong.* The puzzle
does not care. It only counts colors in the right places. But you do.

So the real challenge is not solving the code. It is holding two thoughts at
the same time:

- **"These are just four tiles. Play the feedback."**
- **"These are four parties. Does this make sense?"**

Neither thought is wrong. Neither one alone will solve it. You have to let
both run at once — and then, quietly, decide to trust the tiles.

That is what makes Partymind harder than it looks. Not the math. The instinct.

---

## Parties

Each tile represents one of the eight parties in the Swedish parliament
(*Riksdagen*), in their official colors:

| Tile | Party | Full name |
|------|-------|-----------|
| V    | Vänsterpartiet | The Left Party |
| S    | Socialdemokraterna | The Social Democrats |
| MP   | Miljöpartiet | The Green Party |
| C    | Centerpartiet | The Centre Party |
| L    | Liberalerna | The Liberals |
| KD   | Kristdemokraterna | The Christian Democrats |
| M    | Moderaterna | The Moderate Party |
| SD   | Sverigedemokraterna | The Sweden Democrats |

Four seats are enough for a Swedish government — and that's the puzzle.

---

## Features

- Eight parties, four seats, ten attempts
- Drag-and-drop **or** click-to-place — works with mouse, touch, and stylus
- Classic black/white feedback pegs
- A small coalition comment under each submitted row
- Undo, new game, and a clean dark UI
- Single self-contained HTML file — no build step, no dependencies
- Responsive down to phone size, with `dvh` for stable mobile layout

---

## Running locally

Just open `index.html` in any modern browser. That's it.

To host it yourself on GitHub Pages:

1. Push `index.html` (plus this README and a `LICENSE`) to a public repo.
2. Go to **Settings → Pages**.
3. Set *Source* to **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. Save. Your game will be live at `https://DITTNAMN.github.io/partymind/`
   in about a minute.

---

## Tech

Vanilla JavaScript, no frameworks. Drag-and-drop is implemented with custom
pointer events (`mousedown`/`touchstart` + move tracking) rather than the
HTML5 Drag API, so the same code handles desktop and mobile without the
usual quirks.

The feedback algorithm handles duplicate parties correctly:

```js
function feedback(guess, secret) {
  let black = 0;
  const secretLeft = {}, guessLeft = {};
  for (let i = 0; i < secret.length; i++) {
    if (guess[i] === secret[i]) {
      black++;
    } else {
      secretLeft[secret[i]] = (secretLeft[secret[i]] || 0) + 1;
      guessLeft[guess[i]]   = (guessLeft[guess[i]]   || 0) + 1;
    }
  }
  let white = 0;
  for (const c in guessLeft) {
    white += Math.min(guessLeft[c], secretLeft[c] || 0);
  }
  return { black, white };
}
