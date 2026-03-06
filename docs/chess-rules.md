A comprehensive guide to chess rules as implemented in ClassicChess. Perfect for documentation or for players learning the game.

```markdown
# ♔ Chess Rules: The Complete Guide

*As implemented in ClassicChess v2.0*

## Table of Contents
1. [The Basics](#the-basics)
2. [Piece Movement](#piece-movement)
3. [Special Moves](#special-moves)
4. [Game Endings](#game-endings)
5. [Tournament Rules](#tournament-rules)
6. [Notation](#notation)
7. [FAQ](#faq)

---

## The Basics

### Setup
- **Board**: 8×8 grid, alternating light and dark squares
- **Orientation**: Bottom-right corner must be a light square
- **Pieces per side**: 16 pieces (8 pawns, 2 rooks, 2 knights, 2 bishops, 1 queen, 1 king)

**Initial Position:**
```

a  b  c  d  e  f  g  h
8 ♜ ♞ ♝ ♛ ♚ ♝ ♞ ♜ 8
7 ♟ ♟ ♟ ♟ ♟ ♟ ♟ ♟ 7
6 · · · · · · · · 6
5 · · · · · · · · 5
4 · · · · · · · · 4
3 · · · · · · · · 3
2 ♙ ♙ ♙ ♙ ♙ ♙ ♙ ♙ 2
1 ♖ ♘ ♗ ♕ ♔ ♗ ♘ ♖ 1
a  b  c  d  e  f  g  h

```

### Objective
**Checkmate** the opponent's king – trap it so it cannot escape capture.

### Who Moves First?
**White always moves first.** Players alternate turns.

---

## Piece Movement

### ♙ Pawn
| Aspect | Rule |
|--------|------|
| **First move** | Move forward 1 or 2 squares |
| **Subsequent moves** | Move forward 1 square |
| **Capture** | Diagonally forward 1 square |
| **Cannot** | Move backward or jump over pieces |

**Special**: Pawns are the only pieces that capture differently than they move.

### ♖ Rook
| Aspect | Rule |
|--------|------|
| **Movement** | Any number of squares horizontally or vertically |
| **Capture** | Same as movement |
| **Cannot** | Jump over pieces |

### ♘ Knight
| Aspect | Rule |
|--------|------|
| **Movement** | L-shape: 2 squares in one direction, then 1 perpendicular |
| **Capture** | Same as movement |
| **Special** | Can jump over pieces (only piece with this ability) |

**All possible knight moves:**
```

· · · · · · ·
· · ⬆ ⬆ · · ·
· ⬆ · · ⬆ · ·
· · · ♘ · · ·
· ⬆ · · ⬆ · ·
· · ⬆ ⬆ · · ·
· · · · · · ·

```

### ♗ Bishop
| Aspect | Rule |
|--------|------|
| **Movement** | Any number of squares diagonally |
| **Capture** | Same as movement |
| **Cannot** | Jump over pieces |
| **Note** | Stays on same color squares throughout game |

### ♕ Queen
| Aspect | Rule |
|--------|------|
| **Movement** | Any number of squares horizontally, vertically, or diagonally |
| **Capture** | Same as movement |
| **Cannot** | Jump over pieces |
| **Note** | Most powerful piece (combines rook + bishop) |

### ♔ King
| Aspect | Rule |
|--------|------|
| **Movement** | 1 square in any direction |
| **Capture** | Same as movement |
| **Cannot** | Move into check (square attacked by opponent) |
| **Special** | Can participate in castling (see below) |

---

## Special Moves

### 1. Castling
A double move involving king and rook.

**Conditions:**
- Neither piece has moved
- No pieces between king and rook
- King not in check
- King does not move through or into check
- All squares between must be empty

**How it works:**
```

Kingside (O-O):
Before: ♔ ♖ → After: · ♔ ♖

Queenside (O-O-O):
Before: ♖ · · ♔ → After: · · ♖ ♔

```

**In ClassicChess:** Automatically offered when legal. The king moves two squares toward the rook, and the rook jumps over.

### 2. En Passant ("In Passing")
A special pawn capture available immediately after opponent moves a pawn two squares.

**Conditions:**
- Opponent moves pawn two squares from its starting rank
- It lands beside your pawn
- You must capture **immediately** on the very next turn

**Example:**
```

Before:    After (en passant):
5 · ♙ ·   5 · · ♙
4 ♟ · · → 4 · · ·
3 · · ·   3 · · ·
White pawn on e5, Black plays d7-d5
White captures: exd6 (removes pawn on d5)

```

### 3. Pawn Promotion
When a pawn reaches the opposite end of the board.

**Rules:**
- Must promote to queen, rook, bishop, or knight (same color)
- Cannot remain a pawn
- Cannot promote to king
- Multiple queens possible

**In ClassicChess:** A promotion modal appears automatically. Choose your piece!

---

## Game Endings

### Checkmate ✓
The king is in check and has no legal move to escape.
- **Result**: The checking player wins
- **ClassicChess**: Shows "Checkmate!" with winner announcement

### Stalemate 🤝
The player to move has no legal moves, but their king is NOT in check.
- **Result**: Draw
- **ClassicChess**: Shows "Stalemate — Draw"

### Draw by Agreement 🤝
Both players agree to end the game as a draw.
- **How**: Click "Draw" button, opponent accepts
- **Result**: Draw

### Resignation 🏳️
A player concedes the game.
- **Result**: Opponent wins
- **ClassicChess**: Click "Resign" button

### 50-Move Rule
If 50 moves (50 by each side) occur with:
- No pawn moves
- No captures
- **Result**: Draw (either player can claim)

### Threefold Repetition
If the same position occurs three times with:
- Same player to move
- Same castling rights
- Same en passant possibilities
- **Result**: Draw (automatically detected)

### Insufficient Material
Draw when neither side can checkmate. Examples:
- King vs King
- King + Bishop vs King
- King + Knight vs King
- King + Bishop vs King + Bishop (same color squares)
- **Result**: Draw (automatically detected)

### Time Forfeit ⏰
When using a timer, if a player's clock reaches zero:
- **Result**: Opponent wins
- **ClassicChess**: Clock turns red at 10 seconds, flashes at 0

---

## Tournament Rules

### Time Controls in ClassicChess

| Mode | Initial Time | Typical Use |
|------|--------------|-------------|
| Blitz | 3 or 5 minutes | Fast, exciting games |
| Rapid | 10 or 15 minutes | Club tournaments |
| Classical | 30 minutes | Serious competition |
| Untimed | ∞ | Casual learning |

### Touch-Move Rule
In tournament play:
1. If you touch a piece, you must move it (if legal)
2. If you touch an opponent's piece, you must capture it (if possible)

**ClassicChess:** Follows this rule automatically – selected piece highlights.

### Etiquette
- Say "check" when attacking the king (optional, but traditional)
- Say "adjust" before adjusting pieces (ClassicChess: just tap)
- Shake hands before and after game
- Winner offers handshake, loser accepts graciously

---

## Notation

### Algebraic Notation
The standard way to record chess moves.

**Square names:**
```

Files (columns): a b c d e f g h (left to right from White's perspective)
Ranks (rows):    1 2 3 4 5 6 7 8 (bottom to top from White's perspective)

```

**Piece symbols:**
| Piece | Symbol |
|-------|--------|
| King | K |
| Queen | Q |
| Rook | R |
| Bishop | B |
| Knight | N |
| Pawn | (no letter) |

**Move notation examples:**
| Move | Meaning |
|------|---------|
| e4 | Pawn moves to e4 |
| Nf3 | Knight moves to f3 |
| Bxe5 | Bishop captures on e5 |
| O-O | Kingside castling |
| O-O-O | Queenside castling |
| exd6 | Pawn on e-file captures to d6 |
| Qh4# | Queen to h4 delivers checkmate |
| Nxf7+ | Knight captures on f7 with check |

### In ClassicChess
- Move history shows algebraic notation
- Current move highlighted
- Click any move to review position
- Export complete game as PGN

---

## FAQ

### Q: Can I undo a move?
**A:** Yes! In local games, use the Undo button. In LAN multiplayer, undo is disabled for fairness.

### Q: How do I offer a draw?
**A:** Click the "Draw" button in the top bar (desktop) or sidebar (mobile). Your opponent can accept or decline.

### Q: What's the difference between checkmate and stalemate?
**A:** In checkmate, the king is under attack and cannot escape. In stalemate, the king is not under attack but the player has no legal moves. Stalemate is a draw.

### Q: Can I have more than one queen?
**A:** Yes! When promoting pawns, you can choose queen even if you already have one. Multiple queens are allowed.

### Q: How does the computer know when the game is over?
**A:** ClassicChess checks after every move:
- Is the king in check?
- Are there any legal moves?
- Has the 50-move rule been reached?
- Has the position repeated three times?
- Is there insufficient material?

### Q: Why can't I castle?
**A:** Check these conditions:
- King or rook already moved?
- Pieces between them?
- King in check?
- Would king pass through check?
- All squares empty between?

### Q: What's en passant and why does it exist?
**A:** En passant prevents pawns from avoiding capture by moving two squares. It was introduced in the 15th century and means "in passing" in French.

### Q: Can I change my mind after touching a piece?
**A:** In tournament rules, no – touch-move applies. ClassicChess follows this: once you select a piece, you must move it if you have legal moves.

### Q: How do I export my game?
**A:** Click "PGN" button in sidebar to copy game notation, or "FEN" to copy current position. Paste anywhere to share!

---

## Quick Reference Card

```

MOVEMENT SUMMARY
═══════════════════════════════════
♙ Pawn    : Forward 1 (2 first), capture diagonally
♖ Rook    : Horizontally & vertically (any distance)
♘ Knight  : L-shape (2+1), jumps over pieces
♗ Bishop  : Diagonally (any distance)
♕ Queen   : Any direction (any distance)
♔ King    : One square any direction

SPECIAL MOVES
═══════════════════════════════════
Castling      : King + rook, both unmoved, no checks
En passant    : Capture pawn that moved 2 squares
Promotion     : Pawn reaches last rank → Queen/Rook/Bishop/Knight

DRAW CONDITIONS
═══════════════════════════════════
• Stalemate
• 50 moves without capture/pawn move
• Threefold repetition
• Insufficient material
• Agreement

CHECKMATE
═══════════════════════════════════
King in check + no legal moves = win

```

---

## Learning Resources

- **Practice**: Start with untimed mode, focus on piece development
- **Puzzles**: Set up positions and try to find checkmate
- **Analysis**: Review your games using move history
- **Openings**: Try common first moves: e4, d4, Nf3, c4

---

*ClassicChess implements all FIDE (World Chess Federation) rules as of 2025. For tournament play, always verify current official rules.*

**Happy playing!** ♔
