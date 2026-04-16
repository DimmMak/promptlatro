# PROMPTLATRO Iteration 2 — Stress Test Report

**Simulated:** STANDARD difficulty, Title → Ante 3
**Against:** 8 critical fixes from iteration 1

---

## VERIFIED FIXES (originals now resolved)

- **C1/C2 (Straight/Royal ambiguity):** RESOLVED. Pipeline (4-card, one-of-each-suit, ×4) is now distinct from traditional Straight (5 sequential ranks, ×4). Royal Flush is now explicitly 10-J-Q-K-A same-suit, ×10. Hand Resolution note handles overlap (highest-scoring wins).
- **C3 (example math):** RESOLVED. Example is now labeled PIPELINE, math checks: (25+35+80+70)+30+30+40 = 310 × 4 = 1,240. Clean.
- **C4 (deck/refill undefined):** RESOLVED. Round Mechanics section spells out hand size 7, plays reset per blind, deck reshuffles at blind end.
- **C5 (shop timing):** RESOLVED. "Shop Timing" subsection explicitly says shop opens after every blind; Boss shops get +1 slot.
- **C6 (cash undefined):** RESOLVED. Cash Rewards table with base + overshoot + unused plays + interest. Example math: $4 + $1 + $2 + $1 = $8 matches example. ✓
- **C7 (Ante 1 too hard):** RESOLVED. 60/100/180 is reachable on turn 1 with a single Pair or mid-card plays.
- **C8 (plays reset undefined):** RESOLVED. "PLAYS PER BLIND: 4 (resets at start of every blind)" is explicit.
- **M1 (Joker stacking):** RESOLVED. Order of operations 1-7 is clear and testable.
- **m3 (Press ENTER):** RESOLVED. "Send any message (or `/start`)."
- **m4/m5 (/endless, /change-difficulty):** RESOLVED. Both now in command list. `/sell` also added.
- **m6 (RANKED):** RESOLVED. Removed from difficulty select.
- **m10 (Lil Bowser):** PARTIALLY — now "+1 chip per hand" (trivial effect, acceptable).
- **M15 (Planet roster):** RESOLVED. All 10 hands mapped (Pluto → Eris).
- **Boss clarifications:** Most now have concrete mechanics (Hallucinator -50% ♥ chips, Jailbreaker +50 per face, Rate Limited -5 per card post-mult, Prompt Injection disables 1 joker, Adversary +10% per play).

---

## NEW BUGS (introduced by patches)

1. **Order of operations contradicts example.** Steps say "3. Joker CHIP bonuses → 4. card-level multipliers → 5. hand multiplier → 6. Joker MULT multipliers." But the worked example applies both jokers (Few-Shot Charm, Persona Power) as flat chip adds BEFORE the ×4 — correct. However, if "Royal Treatment" (×1.5 per face card) were active on K♣ + Q♦, where does it slot? Step 4 says card-level mults apply to chips. Example never tests this. Minor ambiguity remains.
2. **"Pipeline (4-suit) cannot be played from same selection as Straight" rule is impossible to violate.** Pipeline is exactly 4 cards one-of-each-suit; Straight is exactly 5 sequential ranks. A selection can't trigger both structurally. The disambiguation clause is dead code, which hints the author was still confused.
3. **"Straight" with mixed suits of 5 cards requires 5 cards but the deck has only 4 suits → at least 2 share a suit.** Fine for a Straight, but makes Pipeline + Straight mutually exclusive-by-card-count (good). HOWEVER: a Straight Flush (5 sequential, same suit) blocks ever qualifying as Pipeline. Consistent, just noting.
4. **Ante 3 Boss = 1000.** With 0-2 jokers and no planets, reaching 1000 in 4 plays on STANDARD is tight. Average strong hand ~250-400. Four Pipelines at 300 each = 1200. Achievable but swingy. Borderline.
5. **Interest cap "max +$5" implies holding $25+.** Boss Blind 1 gives $5 base + overshoot + plays + interest. With $4 start, even perfect play by Ante 1 Boss holds ~$15, so cap rarely hits early. Fine, but max +$3 on overshoot and plays creates possible $14/blind late — deck-upgrade pace may be too fast. Untested.
6. **"Card-level multipliers (e.g., Royal Treatment ×1.5)"** — Royal Treatment is listed under Common Jokers as a joker effect, not a card-level effect. Classification conflict: is it step 3 (joker chips) or step 4 (card-level)? Author conflated two concepts.
7. **Shop item list still shows only one "Joker" price example ($6/$8)** — no explicit pricing table for Common/Rare/Legendary jokers. Economy partially undefined.
8. **Pipeline example uses 5♠ + 7♥ + K♣ + Q♦ = 4 cards** but the hand display shows 7 cards and `/play 1,2,3,5` skips indices 4,6,7. That's fine, but the rule still says "Player selects 1-5 cards." Pipeline being exactly 4 is new info not in that sentence.

---

## STILL BROKEN (originals not fixed)

- **M2 (Claude Opus Mode 3-slot cost):** No clarification added. Unclear what happens when slots are occupied.
- **M3 (Multi-Agent Master "two hands per round"):** Still undefined (per blind? per ante? shared target?).
- **M4/M5 (The Editor / Time Traveler mechanics):** Still vague.
- **M9 ("Context Collapse" 2 cards drawn):** Section was renamed to "hand size capped at 4" — actually FIXED here, retract.
- **M11 (/skip with 0 jokers):** Command now says "cost: $0 if 0 jokers, else lose 1 joker" — FIXED.
- **M12 (/hint cost interaction):** Still says "costs 1 play" but not clarified vs run-level hint cap.
- **M14 (Moon tarot):** Still unspecified whether rank/chips persist.
- **M18 (Aces as face cards):** Still not stated globally.
- **m11 (chips vs score naming):** Example still says "FINAL SCORE: 310 × 4 = 💎 1,240" but also labels 1,240 under "SCORE" cleanly. Mostly fixed.

---

## WORKING WELL

- Pipeline as signature 4-suit hand is pedagogically brilliant — directly reinforces "complete prompt = all 4 components."
- Cash economy now coherent and testable with a worked example.
- Boss blind mechanics (Jailbreaker +50/face, Adversary +10%/play, Rate Limited -5/card post-mult) are now concrete and scorable.
- Round Mechanics block is the single biggest win — unblocks end-to-end simulation.
- Ante curve (60 → 32,000) holds up through simulated Ante 3.
- Planet roster completion gives meta-progression clarity.

---

**Verdict:** Iteration 2 resolved 6 of 8 critical blockers cleanly. Remaining friction is in joker-effect timing (Royal Treatment classification), shop pricing gaps, and a few M-level items still undefined. Game is now simulable end-to-end on STANDARD through Ante 3.
