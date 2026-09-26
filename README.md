<div align="center">

# ♟ Chess Guide v2

### The Royal Game, redesigned.

Tap a piece · Learn its moves · Master the board.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Dependencies](https://img.shields.io/badge/dependencies-none-2f8f79)
![Version](https://img.shields.io/badge/version-2.0-blue)

</div>

**Live demo:** https://4pfanas.github.io/chess-guide-v2/


---

## Table of contents

1. [The idea](#the-idea)
2. [What's new in v2](#whats-new-in-v2)
3. [What it does](#what-it-does)
4. [How it works](#how-it-works)
5. [Piece movement, explained in code](#piece-movement-explained-in-code)
6. [Tech stack](#tech-stack)
7. [Project structure](#project-structure)
8. [Run it locally](#run-it-locally)
9. [Design notes](#design-notes)
10. [Limitations](#limitations)
11. [Roadmap](#roadmap)

---

## The idea

[Version 1](https://github.com/4pfanas/chess-guide) proved that seeing the squares light up is the fastest way to learn how a piece moves. Version 2 keeps that core and rebuilds everything around it: a bolder visual identity, a data-driven movement engine, and content that is folded away until you want it.

The goal is still one page a complete beginner can open, understand in five minutes, and walk away knowing how to play.

## What's new in v2

| | v1 | v2 |
|---|---|---|
| **Typography** | Serif, book-like | Bebas Neue headings, DM Sans body, DM Mono coordinates |
| **Movement logic** | `PIECES` data with `moves()` hard-wired to one demo square each | `moves([rank, file])` takes the square as a parameter, so a piece can be shown from anywhere |
| **Content layout** | Sections laid out in a fixed sequence | Collapsible accordion sections (open by default) |
| **Extra content** | n/a | New **Piece Values** table (Pawn 1 to Queen 9) |
| **Board detail** | Coordinate labels | Coordinates inside each square, plus a distinct "origin" square for the selected piece |
| **Text transitions** | Instant swap | Fade and slide when the explanation changes |

## What it does

- Renders an interactive **8×8 board** with the full starting position.
- Six **piece cards**: choose one and the board shows its **origin square** and every **reachable square** in different colours.
- A description panel explains that piece's movement in plain language, with a smooth fade-in.
- Three **accordion sections** you can open or close:
  - **Core Rules**: check, checkmate, stalemate, capture, promotion, castling, en passant.
  - **Beginner Tips**: centre control, early development, early castling, avoiding wasted moves, checking threats.
  - **Piece Values**: relative strength from pawn (1) to queen (9), king (priceless).

## How it works

**Board construction.** A `START` array defines the position. A double loop creates 64 squares, alternating light and dark, and appends a `<span class="coord">` inside each square with its name (`a1` to `h8`).

**Selecting a piece.** Clicking a card calls `showPiece(name)`:

1. `clearBoard()` removes old `highlight` and `origin` classes and the previous active card.
2. The piece's `moves(from)` function returns a list of `[rank, file]` pairs.
3. `highlightSq()` adds the `highlight` class to each one, silently ignoring anything off the board.
4. The origin square gets its own `origin` class.
5. The description box fades out, swaps its text, and fades back in.

**Accordions.** Sections start open. `toggle(head)` flips an `open` class on the header and a `collapsed` class on the panel below it. CSS handles the animation.

## Piece movement, explained in code

Every piece is described by data plus one small function, so adding or fixing a piece never touches the UI code. In v2 that function takes the origin square as an argument:

```js
knight: {
  title: '♘  Knight — L-Shape, Jumps Over Pieces',
  desc:  'The Knight moves in an "L": 2 squares in one direction, then 1 perpendicular...',
  from:  [4, 4],
  moves: ([r, f]) => [[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]]
                     .map(([dr, df]) => [r + dr, f + df])
}
```

| Piece | Technique |
|---|---|
| **King** | Two nested loops over `-1..1`, skipping `(0,0)` |
| **Queen** | Eight direction vectors, each walked up to 7 squares, then filtered to the board |
| **Rook** | Every square in the same row and column |
| **Bishop** | Four diagonal vectors walked until the edge |
| **Knight** | The eight fixed L-shaped offsets |
| **Pawn** | One or two forward, plus the two forward diagonals for captures |

Each piece is demonstrated from a sensible starting square (`from`) so the pattern is clearly visible on the board.

## Tech stack

| Layer | Choice |
|---|---|
| Markup | HTML5 |
| Styling | CSS3: Grid for the board, transitions for the accordion and text |
| Logic | Vanilla JavaScript (ES6 arrow functions, template literals, destructuring) |
| Pieces | Unicode chess glyphs |
| Fonts | Bebas Neue, DM Sans, DM Mono (Google Fonts) |

## Project structure

```text
chess-guide-v2/
├── index.html   # markup, styles and script in one file
└── README.md
```

## Run it locally

```bash
git clone https://github.com/4pfanas/chess-guide-v2.git
cd chess-guide-v2
open index.html
```

## Design notes

- **Data over branching.** A `PIECES` object plus one generic `showPiece()` beats a long `if/else` chain: adding a piece is adding data.
- **Progressive disclosure.** Rules, tips and values sit in accordions you can fold away, so the board stays the star.
- **Monospaced coordinates** make the board feel precise and help players connect the picture to real chess notation.

## Limitations

- The board shows each piece's *movement pattern* on an otherwise empty board; it does not model blocking, captures or check.
- It is a learning aid, not a playable game.

## Roadmap

- [ ] Make pieces draggable so learners can try moves
- [ ] Add blocking and capture logic to the move calculator
- [ ] Add a "play vs. a beginner bot" mode
- [ ] Mini-quiz: "Which piece can reach this square?"
- [ ] Add keyboard navigation and screen-reader labels

---

<div align="center">

Built by **[Anas Aslam](https://github.com/4pfanas)** · See also [v1](https://github.com/4pfanas/chess-guide).

</div>
