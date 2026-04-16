# 📜 PROMPTLATRO Changelog

## v1.0 — 2026-04-16 — Playable Release

First version verified end-to-end playable through 4 stress-test iterations.

### 🃏 Initial design
- 52-card deck (4 suits × 13 ranks) mapped to prompt engineering components
- 10 poker hand tiers with chips × multiplier scoring
- Common / Rare / Legendary / Secret jokers (real prompt patterns)
- 8 antes with 3 blinds each (Small / Big / Boss)
- Boss Blinds themed as AI failure modes
- Shop with Jokers, Booster Packs, Tarots, Planets
- 3 difficulty modes (Novice / Standard / Expert)

### 🚨 Critical fixes (from iteration 1 stress test)
- **Renamed "Straight" → "Pipeline"** for the 4-card one-of-each-suit hand
- **Added traditional Straight** (5 sequential ranks, mixed suits)
- **Differentiated Royal Flush** from Straight Flush (200 chips ×10 mult, requires 10-J-Q-K-A same suit)
- **Added Round Mechanics section** covering deck refill, plays/discards reset, draw behavior
- **Added Cash Rewards formula** (base + overshoot + unused plays + interest)
- **Lowered Ante 1 targets** (100→60, 150→100, 250→180) for fair STANDARD start
- **Clarified shop timing** — opens after every blind, not boss-only
- **Fixed example hand** — relabeled as Pipeline so math is internally consistent
- **Stated plays/discards reset per blind** explicitly

### ⚠️ Boss blind mechanics defined
- The Vague User: removes first ♠️ from selection, remaining cards re-resolve
- The Hallucinator: ♥️ cards have ×0.5 chip value
- The Jailbreaker: +50 to target per face card played
- Rate Limited: -5 from final score per card played
- Context Collapse: hand size capped at 4
- The Specialist Trap: Flush/Four-of-a-Kind get ×0.5 mult
- The Claude Refusal: Aces cannot be played this blind
- Prompt Injection: 1 random Joker disabled (restored after blind)
- Context Window Full: max 4 cards per play
- The Adversary: target +10% after each play

### 🃏 Joker stacking rules
- 8-step order of operations defined
- Multiplicative MULT stacking specified
- Same-named jokers stack
- Multi-slot joker purchase rules clarified
- `/sell` command added (returns $3, frees slot)

### 🎲 Edge cases resolved (iterations 2-4)
- Aces are NOT face cards (global rule)
- 5-card no-name hands score as High Card
- Boss blind selection: random from current Ante's tier, no repeats
- Royal Treatment ×1.5 ordering: applied at step 1 (card-chip mult)
- RAG Enhancer + Context Collapse: hand size +2, but boss caps still apply
- Interest calculated after base + bonuses
- Booster packs can grow deck past 52 (soft cap 80)

### 🎮 Commands added
- `/sell [slot]` — sell joker for $3
- `/endless` — continue past Ante 8
- `/change-difficulty` — restart on different difficulty
- `/start` — begin from title screen

### 🔧 Polish
- Title screen: "Press ENTER" → "Send any message (or `/start`)" for chat compatibility
- Removed RANKED difficulty (timer unenforceable in chat)
- Lil Bowser joker now has +1 chip per hand (was nothing)
- Full Planet roster listed (was 2 + "etc.")
- Shop pricing tiers added (Common $4-6, Rare $7-9, Legendary $10-12)
- Bjork's Whisper clarified ($desirable difficulty$ → ×2 cash on hard blinds)

---

## Pre-v1.0 — Stress test artifacts

Preserved in repo for transparency:
- `temporary.md` — Iteration 1: 8 critical / 19 medium / 15 minor issues
- `iteration2.md` — Verification that critical fixes worked
- `iteration3.md` — Edge case stress test
- `iteration4.md` — Full playthrough proving Ante 1 → Boss is playable

---

🃏🎰 Built in one Claude Code session. Designed for play, not study.
