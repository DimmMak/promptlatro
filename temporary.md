# PROMPTLATRO SKILL.md — Stress-Test Report

**Simulated playthrough:** STANDARD difficulty, attempting Title Screen → Ante 3
**Source file:** `/Users/danny/Desktop/CLAUDE CODE/promptlatro/SKILL.md`

---

## CRITICAL FIXES (game-breaking — must fix before playable)

### C1. "Straight" definition is mathematically impossible as written
- **Location:** Line 169 (hand table) + Line 176 (clarifying note) + Lines 450–466 (example flow)
- **Problem:** A "Straight" is defined as "one of each suit (♠♥♦♣)" — that's 4 suits, which means exactly 4 cards. But the scoring example on lines 454–466 plays 4 cards (5♠, 7♥, K♣, Q♦) and calls it a Straight. Meanwhile, the round structure on line 326 states "Player selects cards to play (1–5 cards per hand)" and Flush is defined as "5 cards of the same suit" (line 178). This creates a fundamental contradiction:
  - If hands are 5 cards, a "Straight" of 4 suits is impossible (4 ≠ 5, and pigeonhole forces a duplicate suit, breaking the "one of each" rule).
  - If Straights are 4 cards, Full House (requires 5), Four of a Kind (requires 5), Straight Flush and Royal Flush (both require 5) can never coexist with a 4-card Straight in a unified rule.
  - Also: a Straight-Flush is literally impossible — you cannot have 5 of the same suit AND one of each suit.
- **Fix:** Pick a model and commit. Options:
  1. Redefine Straight as "5 cards, one of each suit + one wild/repeat" (but then it overlaps Flush logic).
  2. Make Straight = "any 4 cards, one per suit" and explicitly state it is the only 4-card hand. Then drop Straight Flush / Royal Flush or redefine them.
  3. Use traditional sequential-rank Straights (2-3-4-5-6 across suits) and let Straight Flush mean "5 sequential of one suit." This is cleanest and matches Balatro.
- **Severity:** CRITICAL — the example playthrough in the SKILL itself plays a "Straight" that cannot fit the stated rules alongside other hand types.

### C2. Royal Flush and Straight Flush are indistinguishable
- **Location:** Lines 172–173
- **Problem:** Both hands list 100 chips and ×8 mult with no differentiation in trigger condition or reward. In traditional poker, a Royal Flush is A-K-Q-J-10 of one suit. Here neither is defined, and their stats are identical. Also, if "Flush = 5 of one suit" (line 178), and Straight = "one of each suit" (line 176), then Straight Flush is impossible by construction (see C1).
- **Fix:** Either remove Royal Flush (or Straight Flush) or define distinct triggers (e.g., "Royal Flush = J♠ Q♠ K♠ A♠ + one more ♠"). Give them different rewards.
- **Severity:** CRITICAL

### C3. Scoring math in the example is internally inconsistent
- **Location:** Lines 450–477
- **Problem:** Breakdown line says "Card chips: 210, Hand bonus: +30, Joker chips: +70, Multiplier: ×4, TOTAL: 1,240." Check: (210 + 30 + 70) × 4 = 310 × 4 = 1,240. That works. BUT the text above says "Card Chips: 25 + 35 + 80 + 70 = 210" — which is correct — yet this is a "Straight" per C1's broken definition and also the player typed `/play 1,2,3,5` (indices 1, 2, 3, 5 = 5♠, 7♥, K♣, Q♦). That's 4 cards. The game loop says 1–5 cards, fine. But nowhere is it stated that hands <5 cards can qualify as Straight — the Straight rule is "one of each suit" which reads as exactly 4 cards. Players will not know whether 4 cards is legal, whether 5 with a duplicate suit still counts, or whether they must always play 4.
- **Fix:** State minimum/maximum card counts for every hand type explicitly. Add a table column: "Cards required."
- **Severity:** CRITICAL

### C4. "Play 4 hands per round" vs "1–5 cards per hand" vs deck depletion
- **Location:** Lines 322–328
- **Problem:** "Player selects cards to play (1–5 cards per hand)" + "If out of plays (4 per round): LOSE." The player starts with 7 cards. They can play up to 4 hands of up to 5 cards each = up to 20 cards played, but the deck is only 52 cards and the hand is 7. After playing cards, are played cards removed from the deck? Do they refill the hand to 7 from the remaining deck? What happens when the deck runs out mid-round? None of this is specified.
- **Fix:** Specify: (a) after a play, hand refills from deck to 7; (b) played cards go to discard and are reshuffled at round end OR are gone for the run; (c) what happens if deck empties mid-round.
- **Severity:** CRITICAL — without this, the game cannot actually run multiple rounds.

### C5. Blinds per Ante vs Boss-only scoring table mismatch
- **Location:** Lines 311–314 (Round Structure) vs Lines 496–508 (Scoring Targets) vs Line 498
- **Problem:** The round structure lists THREE blinds per ante: Small → Big → Boss. The scoring table confirms this with three columns. BUT the player-flow example on line 426 jumps straight to "ANTE 2 — BIG BLIND" and the boss preview on line 488 says "Next: 👹 BOSS BLIND" with no Small Blind shown. Also, the shop (line 349) says "After beating each Boss Blind" — but the commands section (line 411) says `/shop` "only available between antes." What about between Small Blind and Big Blind? Is there a shop there? In Balatro there is.
- **Fix:** Clarify whether shop appears between every blind (Balatro-style) or only after Boss. Reconcile with command description.
- **Severity:** CRITICAL (affects the entire economy)

### C6. Cash earnings never defined
- **Location:** Line 486 ("Cash earned: 💰 $8") and throughout
- **Problem:** Nowhere does the SKILL define how much cash is earned per blind cleared. $8 appears in the example, $12 in the shop example, starting cash varies by difficulty, but there is no formula. Is it a flat reward per blind? Does it scale with overkill? Are there interest mechanics (like Balatro's $1 per $5 held)?
- **Fix:** Add a "Cash Rewards" subsection with per-blind payouts, overkill bonuses, and whether interest accrues.
- **Severity:** CRITICAL — shop economy is undecidable without this.

### C7. Ante 1 Small Blind (target 100) is extremely difficult with starting hand
- **Location:** Line 500 + line 322
- **Problem:** STANDARD starts with 0 jokers, 7-card hand. The smallest-valued card is 10 chips, the highest is 100. A Pair (×2 mult, +10 chip bonus) of low cards scores roughly (10+15+10) × 2 = 70 — below the 100 target. To hit 100, the player likely needs a face card + a good hand type on turn 1. With random draws, that's not guaranteed in 4 plays. Not impossible, but extremely swingy for turn 1.
- **Fix:** Either lower Ante 1 Small Blind to ~60–80, or guarantee starting hand has at least one face card, or give +1 "starter Joker" on Standard.
- **Severity:** CRITICAL (bad first impression, many players will lose Ante 1 Small Blind)

### C8. "Out of plays" loss condition undefined for multi-blind rounds
- **Location:** Line 327 ("out of plays (4 per round): LOSE")
- **Problem:** Do "plays remaining" reset between Small/Big/Boss blinds, or across the whole Ante? Same question for discards. Example on line 430 says "Plays remaining: 4, Discards remaining: 3" — suggesting per-blind reset. Not stated explicitly.
- **Fix:** State "plays and discards reset at the start of every blind."
- **Severity:** CRITICAL

---

## MEDIUM FIXES (UX — should fix)

### M1. Joker stacking is undefined
- **Location:** Lines 182–235
- **Problem:** If I hold "Few-Shot Charm" (+30 per ♦️) and "Format Enforcer" (+20 per ♥️) and play a hand with 2♦ + 3♥, do both fire? Do duplicates of the same joker stack? What's the order of operations for chip bonuses vs mult bonuses vs ×multipliers? "Royal Treatment" says face cards "give ×1.5 chips" — is that a per-card multiplier applied before or after joker chip bonuses?
- **Fix:** Add a "Joker Stacking & Order of Operations" section. Specify: (1) all chip bonuses sum first, (2) then hand mult applies, (3) then ×multipliers apply left-to-right in joker slot order.
- **Severity:** MEDIUM (the only example in the SKILL dodges this)

### M2. "Claude Opus Mode" Joker costs 3 slots — mechanic unclear
- **Location:** Line 218
- **Problem:** "×4 all final scores (costs 3 joker slots)." Does buying it auto-delete 2 other jokers? Can you only buy it if you have 3 open slots? What happens when inventory is full?
- **Fix:** Clarify purchase rules for multi-slot jokers.
- **Severity:** MEDIUM

### M3. "Multi-Agent Master" — two hands per round, timing unclear
- **Location:** Line 217
- **Problem:** Does "play TWO hands per round" mean per blind (doubling plays from 4 to 8)? Per ante? Do scores sum toward the target? Do discards also double?
- **Fix:** Specify.
- **Severity:** MEDIUM

### M4. "The Editor" — swap one card per hand
- **Location:** Line 208
- **Problem:** Swap with what? A card from the deck? A new random card? Does the swap consume a play/discard?
- **Fix:** Specify source of swapped card and cost.
- **Severity:** MEDIUM

### M5. "The Time Traveler" — undo last hand
- **Location:** Line 219
- **Problem:** Does this restore plays/discards? What happens if the undo would revive a hand that already triggered "The Vague User" discard?
- **Fix:** Specify undo scope (score only vs full state rewind).
- **Severity:** MEDIUM

### M6. Boss Blind "Rate Limited" interaction with scoring
- **Location:** Lines 266–267
- **Problem:** "Each card played costs 5 chips." Is the cost deducted from score, from cash, or from card chip value? If score, is it subtracted before or after multiplier?
- **Fix:** Specify timing and target.
- **Severity:** MEDIUM

### M7. Boss Blind "The Jailbreaker" — boss plays 1 card
- **Location:** Lines 263–264
- **Problem:** "Boss plays 1 card AGAINST you." What does this mean? Reduces your score by that card's chips? Adds to a target? Occupies a slot in your hand?
- **Fix:** Define the mechanic.
- **Severity:** MEDIUM

### M8. Boss Blind "The Adversary" — boss plays a complete hand
- **Location:** Lines 292–293
- **Problem:** Same issue as M7 but scaled up. Also conflicts with roguelike solitaire framing — you're not playing against an opponent elsewhere in the game.
- **Fix:** Define exact scoring interaction.
- **Severity:** MEDIUM

### M9. Boss Blind "Context Collapse" — 2 cards drawn per round
- **Location:** Lines 272–273
- **Problem:** Does this mean hand size is 2, or refill rate is 2? If hand size 2, no Straight/Flush/Full-House is possible.
- **Fix:** Clarify.
- **Severity:** MEDIUM

### M10. Boss Blind "The Claude Refusal" — which combos are blocked?
- **Location:** Lines 278–279
- **Problem:** "Certain card combos blocked mid-play" but commands are "only between hands" (line 398). How does the block surface? Randomly? Which combos?
- **Fix:** Specify or remove the "mid-play" phrasing.
- **Severity:** MEDIUM

### M11. "/skip" command — lose 2 jokers
- **Location:** Line 410
- **Problem:** Standard starts with 0 jokers. What happens if you `/skip` with fewer than 2 jokers? Does it refuse, take what you have, or apply cash penalty instead?
- **Fix:** Handle the zero-joker edge case.
- **Severity:** MEDIUM

### M12. "/hint" costs 1 play — Ante conflict
- **Location:** Lines 413 + 569–586
- **Problem:** You have 4 plays per blind. Using `/hint` 4 times = auto-loss. Also Standard allows only "3 hints per run" (line 663). So hint is bounded twice (per-run AND per-play-cost). Is the per-play cost still charged if you're out of run-level hints?
- **Fix:** Clarify interaction.
- **Severity:** MEDIUM

### M13. Booster pack adds cards to deck but deck is fixed at 52
- **Location:** Line 371 + line 72
- **Problem:** "5 random cards to permanently add to your deck." If deck is always 52, where do new cards fit? Do duplicates now exist (two 5♠)?
- **Fix:** State deck can grow past 52; duplicates are legal; note any cap.
- **Severity:** MEDIUM

### M14. "The Moon" tarot — transform card into another suit
- **Location:** Line 383
- **Problem:** Does the transformed card retain its rank/chips? Is the new suit random or player-chosen?
- **Fix:** Specify.
- **Severity:** MEDIUM

### M15. Planets reference but roster incomplete
- **Location:** Lines 388–392
- **Problem:** "🪐 Pluto (High Card) 🌌 Mercury (Pair) ... etc." Only 2 planets listed with "etc." — player can't know the full list. Also there are 10 hand types but only 8 classical planets.
- **Fix:** List all planet → hand-type mappings.
- **Severity:** MEDIUM

### M16. "/ask" command + "Auto-advance" rule conflict
- **Location:** Line 412 + Line 692
- **Problem:** "/ask [question] — Ask Claude anything mid-run, auto-resume" is a mid-game tutorial, but rule #4 says "NO TUTORIALS MID-GAME." Rule #1 says "AUTO-ADVANCE AFTER EACH HAND. Don't wait for the player to confirm." How does `/ask` fit if auto-advance is strict?
- **Fix:** Clarify: `/ask` pauses auto-advance, answers briefly, resumes.
- **Severity:** MEDIUM

### M17. RANKED difficulty — 60-second timer unenforceable in chat
- **Location:** Line 682
- **Problem:** "Timed rounds (60 seconds per hand)." In a chat interface without real-time enforcement, who enforces the timer? Does Claude count wall-clock between messages? Hand latency is unpredictable.
- **Fix:** Either drop the timer or define "time" as "number of Claude responses" or "message count."
- **Severity:** MEDIUM

### M18. Face-card threshold for "Persona Power" ambiguous
- **Location:** Line 192
- **Problem:** "+40 chips if any ♣️ is a face card." Aces — are they face cards? In the deck listing (line 91), only J/Q/K are labeled "(face card)" and Ace is called "high power, high cost." So Ace ≠ face. Worth stating once globally.
- **Fix:** Add "Aces are not face cards unless specified" to a definitions section.
- **Severity:** MEDIUM

### M19. Hand size reduction boss "Short Attention" specifics
- **Location:** Lines 249–250
- **Problem:** "Hand size reduced to 3." Still 5-card max play? If so, most hand types impossible. Does the cap apply to display or to play?
- **Fix:** Specify.
- **Severity:** MEDIUM

---

## MINOR FIXES (polish)

### m1. Inconsistent emoji/label in card suits
- **Location:** Lines 38–41, 74, 94, 114, 134
- **Problem:** Header uses "♠️ CONTEXT" (with emoji variation selector) but inline card listings use "2♠" without variation selector. Rendering will differ across terminals.
- **Fix:** Pick one and stick.
- **Severity:** MINOR

### m2. "Cash earned: $8" is suspiciously flat
- **Location:** Line 486
- **Problem:** Crushing a blind by 4× should reward more than a narrow win. Balatro rewards "overshoot money" and "hands remaining bonus" and "interest." None present.
- **Fix:** Add overshoot bonus.
- **Severity:** MINOR

### m3. "Press ENTER to start" in a chat interface
- **Location:** Line 48
- **Problem:** There is no ENTER to press; any message starts the run.
- **Fix:** "Send any message to start your run" or "Type `/start`."
- **Severity:** MINOR

### m4. `/change difficulty` command undocumented
- **Location:** Line 564
- **Problem:** Lose screen references `/change difficulty` but it's not in the command list (lines 399–417).
- **Fix:** Add to command list.
- **Severity:** MINOR

### m5. `/endless` command undocumented
- **Location:** Line 539
- **Problem:** Win screen references `/endless` but it's not in the command list.
- **Fix:** Add.
- **Severity:** MINOR

### m6. Difficulty [4] RANKED added but unsupported in chat
- **Location:** Line 65 + 679–686
- **Problem:** Leaderboards and seed-based reproducibility are not implementable in a per-session Claude run.
- **Fix:** Either remove the option or label as "planned / experimental."
- **Severity:** MINOR

### m7. Joker slot count inconsistency
- **Location:** Line 184 ("up to 5 Jokers") vs Line 410 (`/skip` loses 2 jokers) vs Line 218 ("costs 3 joker slots") vs Line 655 (Novice starts with 1 joker)
- **Problem:** Basic numbers check out, but no statement of what happens if shop offers a joker when inventory is full. Can you sell jokers for cash? No mention.
- **Fix:** Add "Managing Jokers" paragraph.
- **Severity:** MINOR

### m8. Title-screen "show ONCE per session" but roguelike is one run per session?
- **Location:** Line 20
- **Problem:** If a session may contain multiple runs (`/restart`), should title re-show? Unclear.
- **Fix:** Specify.
- **Severity:** MINOR

### m9. "Bjork" vs "Björk" and investor nods
- **Location:** Line 228, 232–234
- **Problem:** "Bjork's Whisper" — likely "Björk" or Bjarke — unclear. "Dr. Druck" (Druckenmiller?), "Dr. Klarman," "The Cathie Wood" — the flavor is cute, but "Tulving" / "Miller" / "Simon" are cognitive-science names mixed with finance names with no obvious rule. New players won't get the references; that's fine for easter eggs, but ensure effects are testable.
- **Fix:** Cosmetic only; works as-is.
- **Severity:** MINOR

### m10. "Lil Bowser" joker does nothing
- **Location:** Line 234
- **Problem:** "Does nothing visible. Vibes only." — if it truly does nothing, why occupy a slot? Cute gag but will frustrate.
- **Fix:** Give it a tiny hidden effect (e.g., +1 chip per hand) or label as cosmetic.
- **Severity:** MINOR

### m11. Score breakdown unit inconsistency
- **Location:** Lines 468–477
- **Problem:** "💎 1,240 chips!" after multiplier — but the target is also in chips. Calling the post-multiplier total "chips" conflates base-chips with final-score. Balatro distinguishes "chips × mult = score."
- **Fix:** Rename "FINAL" to "SCORE" and keep "chips" for pre-mult.
- **Severity:** MINOR

### m12. "Discards remaining: 3" vs rule "DISCARD up to 3 times per round"
- **Location:** Lines 325 + 432
- **Problem:** Consistent (good), but not stated whether a discard can be 1-5 cards at once, or only 1 card per discard.
- **Fix:** Clarify (Balatro allows up to 5).
- **Severity:** MINOR

### m13. Joker list vs suit-only triggers
- **Location:** Line 190 ("+30 chips when you play any ♦️ card")
- **Problem:** "Any" suggests at least one ♦️ in the hand triggers +30 total. But "Format Enforcer" says "+20 chips for each ♥️ card" — explicitly per card. Inconsistency is confusing.
- **Fix:** Rewrite for uniform phrasing: "+X per card" or "+X if ≥1 card."
- **Severity:** MINOR

### m14. Learning-moment triggers vs rule "not every hand"
- **Location:** Line 696
- **Problem:** Only 3 learning moments defined (lines 596–642). For a run of 40+ hands, most plays will have no tip. Fine for MVP, but the SKILL should state "add more as we author them."
- **Fix:** Note it's an authoring backlog.
- **Severity:** MINOR

### m15. Save state schema mentions "hand_type_counts" but never used
- **Location:** Lines 751–756
- **Problem:** Field is tracked but never consumed anywhere (no joker/tarot references it). Dead field.
- **Fix:** Either remove or tie to "Template King" joker tracking.
- **Severity:** MINOR

---

## WORKING WELL (don't touch)

- **Suit → concept mapping (♠Context, ♥Instruction, ♦Example, ♣Persona)** is clean, memorable, and pedagogically sound.
- **Deck composition (52 cards, 4×13)** is correct: 4 suits × 13 ranks = 52. Chip values scale monotonically 10→100 with face cards and aces at 60/70/80/100, which is a fun curve.
- **Ante progression scale** (100 → 32,000 over 8 antes, ~2× per ante) is a reasonable Balatro-style exponential.
- **Boss blind theming** (AI failure modes) is the standout pedagogical hook — every name teaches something real.
- **Difficulty modifier structure** (Novice/Standard/Expert clearly differentiated on target, cash, jokers, hints).
- **Learning moments** for real-world tips are genuinely helpful and on-brand.
- **Command list** (setting aside m4/m5) is comprehensive and well-prefixed.
- **Save-state JSON shape** is simple and serializable.
- **The "PLAYING = LEARNING"** translation table (lines 766–777) is the best part of the document; keep as-is.

---

## SUGGESTED ADDITIONS (nice-to-haves)

1. **"Starting Hand" section** — spell out: deck shuffled, top 7 drawn, rest of run state initialized to defaults.
2. **Card removal mechanic** — how does "The Tower" tarot interact with deck count? Can you drop below 40 cards? Any minimum?
3. **Visual ASCII card art** — even minimal, makes the game feel more Balatro-y in a text UI.
4. **"Voucher" shop tier** — Balatro has persistent vouchers; adding one or two would deepen the meta.
5. **Seeded runs (Standard too)** — "Your run seed: PROMPT-7F4A" lets players share.
6. **Joker synergy tips in shop** — when offering a joker, note if it synergizes with current inventory.
7. **End-of-ante recap** — a short "this ante you played X hands, best was Y" for rhythm.
8. **`/sell [joker_slot]`** — standard Balatro lets you sell jokers. Currently no way to free slots.
9. **Interest mechanic** — "Earn $1 per $5 held, cap $5" would give the economy depth.
10. **Explicit "new-run init" block** — what resets on `/restart` vs `/quit` vs title screen.
11. **"Pinned hand" / mark-for-keep discard** — a common Balatro-style QoL.
12. **Deck-view formatter** — `/deck` command has no example output. Show 52-card grid.
13. **Hand-type frequency meter** — "You've played 3 Flushes, 1 Straight" at any time.
14. **Boss-blind preview at Small Blind** — Balatro shows the upcoming boss so you can plan; SKILL never says whether Boss is revealed early.
15. **Pity timer for rare jokers** — guarantee at least one Rare after N shops.

---

## Summary

**Blocker count:** 8 critical issues. The game cannot be run end-to-end as written because:
1. "Straight" definition contradicts Flush/Full-House/Royal-Flush co-existence.
2. Scoring example plays an impossible hand.
3. Deck/hand refill mechanics are undefined.
4. Cash rewards are undefined (breaks shop economy).
5. Shop timing between blinds is ambiguous.
6. Ante-1 Small Blind target is too high for STANDARD starting state.

**Recommended first edit:** Rewrite the "Poker Hands" section with explicit card-count requirements per hand type, then add a "Round Mechanics" section covering deck, draw, discard, and cash. Those two edits alone unblock most of the CRITICAL list.
