# PROMPTLATRO — Iteration 3 Edge Case Findings

## EDGE CASE BUGS (must fix)

1. **5 cards = 1-of-each-suit + 1 duplicate suit is undefined.** Spec says Pipeline requires EXACTLY 4 cards. So 5 mixed-suit cards fall through to Flush-check (fails, no 5-same-suit) → defaults to High Card / Pair / Straight. No rule says "drop the dupe suit and score Pipeline from 4." Players will feel cheated. Add rule: "If 5 played and no higher hand applies, best 4-subset Pipeline is allowed" OR explicitly forbid.

2. **Royal Flush vs Straight Flush collision.** 10-J-Q-K-A same suit qualifies for both. Spec says "highest-scoring applies" — Royal (×10, 200) beats Straight Flush (×8, 100). OK, but this must be stated explicitly in Hand Resolution section; currently only implied.

3. **Royal Treatment order-of-ops is broken.** Step 3 applies joker CHIP bonuses, step 4 applies card-level multipliers like Royal Treatment ×1.5. But ×1.5 on face-card CHIPS means it should scale card values, not the post-joker total. Spec is ambiguous: does ×1.5 multiply the face card's base chip (60 → 90) BEFORE joker chips, or scale the running total AFTER joker chips? Current step order applies it AFTER joker chips, inflating joker contributions. Fix: clarify Royal Treatment modifies card value at step 1, not a separate step 4.

4. **Claude Opus Mode + 5 jokers = unresolved.** Costs 3 slots. If inventory has 5, buying it needs 3 free slots. Spec says "If inventory full … must /sell one first" — but you'd need to sell THREE. Not stated. Also: does it occupy 3 contiguous slots or 3 of 5?

5. **The Hallucinator timing.** "♥️ cards have -50% chip value" — applied at which step? If before step 3 joker chips, Format Enforcer (+20 per ♥) still scales correctly. If after, joker bonuses based on ♥ count are unaffected but card chips are halved post-bonus. Spec silent. Default should be: modify at step 1 (card chips), before anything else.

6. **Context Collapse + RAG Enhancer conflict.** Hand size capped at 4 vs +2 cards at round start. Does RAG Enhancer add to the capped 4 (= 6) or is the cap absolute? Spec doesn't say. Boss effects should override jokers (cap = 4 final) for consistency.

7. **$0 cash + full joker inventory + shop.** /skip spec: "cost $0 if 0 jokers, else lose 1 joker". Player with $0, 5 jokers, can still /skip by losing one. Fine. But if shop appears and only joker-purchase options, player has no cash — skip is always available, so no soft-lock. Confirm in spec.

8. **Booster pack dupes scoring as Pair.** Two K♠ in deck. If both drawn and played, they share rank (K) and suit (♠) — standard poker treats identical cards as a pair by rank. Spec allows dupes but doesn't say what happens. Two K♠ = Pair by rank = legal. Confirm.

## AMBIGUITIES (would clarify)

- **Single card played with duplicate rank in unplayed hand**: correctly High Card per spec (only played cards score). State this explicitly.
- **Ace-low straight A-2-3-4-5**: spec line 181 confirms both. Good.
- **3-of-a-kind + 1 other on 4 cards**: if the 4th has a unique suit AND the 3 cover the other 3 suits, it's both Three-of-a-Kind (×3) and Pipeline (×4). Highest wins = Pipeline. Worth an example.
- **The Jailbreaker + Ace**: "M18 fix" referenced but not in this spec — add "Aces do NOT count as face cards for Jailbreaker" inline.
- **Overshoot bonus formula**: Boss 250 cleared with 1000 = 300% over = +$3 (capped). Matches "max +$3". OK but formula ("per 100% over") suggests +$3 at 300% exactly — edge case at 301% still +$3. Fine.
- **Joker MULT stacking order**: step 6 is left-to-right; does Chain-of-Thought ×3 chain with Claude Opus ×4? Multiplicative (×12) or additive? Assume multiplicative; state it.

## WORKING

- Pipeline strictly 4 cards, prevents overlap with Straight.
- Straight allows both A-low and A-high.
- Same-named jokers stack (Few-Shot Charm ×2 = +60/♦).
- Deck composition persists across blinds; reshuffles with new cards each blind.
- Cash rewards formula is complete and testable.
- Highest-scoring hand wins on multi-qualification.
- Boss penalties applied LAST (step 7) — consistent ordering.
- Shop appears after every blind (Small/Big/Boss), Boss shop +1 slot.
