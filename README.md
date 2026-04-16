# 🃏🎰 PROMPTLATRO

> **Learn prompt engineering by playing poker.**
> A roguelike deckbuilder inspired by [Balatro](https://www.playbalatro.com/), where every card is a real prompt engineering pattern and every boss is an AI failure mode.

```
♠️ CONTEXT       — background, user intent, constraints
♥️ INSTRUCTION   — task directions, output format
♦️ EXAMPLE       — demonstrations, few-shot patterns
♣️ PERSONA       — roles, style, voice
```

Build hands. Stack patterns. Beat the Boss AI.

---

## 🎯 Why this exists

Nobody wants to study prompt engineering. Everyone wants to win.

PROMPTLATRO disguises a master class in prompt patterns as a roguelike deckbuilder. After 30-50 runs, players have internalized:
- ✅ Context-first prompting
- ✅ Chain-of-thought reasoning
- ✅ Few-shot pattern matching
- ✅ Persona-based prompting
- ✅ Guardrails and adversarial robustness
- ✅ Token efficiency
- ✅ Multi-agent orchestration

They didn't *study* prompt engineering. They *played* it.

---

## 🎮 How to play

PROMPTLATRO is a Claude `.skill` file. It runs entirely inside a Claude Code session — no install, no server, no UI framework.

### Option 1: Drop into your skills directory
```bash
cp SKILL.md ~/.claude/skills/promptlatro/SKILL.md
```
Then in any Claude session, invoke it via `/promptlatro` or `Skill: promptlatro`.

### Option 2: Paste as system prompt
Copy the contents of `SKILL.md` into Claude's system prompt and start chatting.

### Option 3: Standalone playthrough
Open Claude, paste this:
> Read `/path/to/promptlatro/SKILL.md` and run it as a game.

---

## 🃏 Core mechanics

| Mechanic | What it teaches |
|---|---|
| 4 suits = 4 prompt components | Context / Instruction / Example / Persona structure |
| Pipeline hand (one of each suit) | Complete production-grade prompts |
| Flush (5 of one suit) | Going deep in one dimension |
| Jokers as power-ups | Real patterns: Chain-of-Thought, Few-Shot, Self-Consistency, ReAct, Constitutional AI, RAG |
| Boss Blinds | Real AI failure modes: Hallucination, Jailbreaking, Rate Limits, Refusals, Context Collapse, Prompt Injection |
| Roguelike replay | Mastery through repetition |

### Hand tiers (chips × multiplier)

```
HAND              | CARDS | BASE CHIPS | BASE MULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
High Card         |   1   |     5      |    ×1
Pair              |   2   |    10      |    ×2
Two Pair          |   4   |    20      |    ×2
Three of a Kind   |   3   |    30      |    ×3
Pipeline (4-suit) |   4   |    30      |    ×4   ← Promptlatro signature
Flush             |   5   |    35      |    ×4
Full House        |   5   |    40      |    ×4
Four of a Kind    |   4   |    60      |    ×7
Straight          |   5   |    30      |    ×4
Straight Flush    |   5   |   100      |    ×8
Royal Flush       |   5   |   200      |   ×10
```

---

## 🎲 Difficulty modes

| Mode | Targets | Cash | Jokers | Hints |
|---|---|---|---|---|
| 🟢 NOVICE | ×0.75 | $8 | 1 | unlimited |
| 🟡 STANDARD | ×1.0 | $4 | 0 | 3/run |
| 🔴 EXPERT | ×1.25 | $0 | 0 | 1/run |

---

## 🛠️ Repo contents

```
promptlatro/
├── SKILL.md          # The actual game (drop into Claude)
├── README.md         # This file
├── CHANGELOG.md      # Patch history
├── temporary.md      # Iteration 1 stress test (8 critical issues found)
├── iteration2.md     # Verified fixes
├── iteration3.md     # Edge case stress test
└── iteration4.md     # Full playthrough verification
```

---

## 📜 Provenance

Built in a single Claude Code session as part of an exploration into using `.skill` files as a rapid prototyping format. The game went through 4 stress-test iterations before reaching playable state — each iteration's report is preserved for transparency.

**Sister project:** [CodeRecall](https://github.com/DimmMak/coderecall) — SQL/Python drill game using BibleMemory Pro's first-letter recall mechanic.

---

## 🎰 Status

**v1.0 — Playable.** Cleared end-to-end Ante 1 → Boss verification. Open to playtest feedback.

🃏🎰🔥
