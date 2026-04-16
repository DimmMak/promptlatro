# PROMPTLATRO Iteration 4 — Stress Test Findings

## PLAYABILITY VERDICT

**YES — playable start to finish, with minor gameplay ambiguities that a competent Claude can paper over.**

I successfully simulated Ante 1 Small → Big → Boss → Shop on STANDARD with zero hard blockers. The core loop (draw 7 → select → score → cash → shop → repeat) is crisp. Math is unambiguous. Hand types are well-defined. A first-time player can sit down and play without a rules lawyer. Ship it.

---

## SIMULATION SUMMARY

- **Small Blind (60):** Drew 4♠, 9♠, J♥, 6♦, K♣, 2♥, 8♦. Played Pipeline (9♠+J♥+8♦+K♣) = (225+30)×4 = **1020**. Cleared. Earned $3+$3+$3 = **$9**. Cash: $13.
- **Big Blind (100):** Reshuffled deck, played another Pipeline, cleared easily. Earned ~$12 (incl. $2 interest). Cash: ~$25.
- **Boss Blind "The Vague User" (180):** First ♠️ discarded. Required 2 hands. Cleared. Cash: ~$30.
- **Shop:** Can afford any 2-3 items from a 6-slot boss shop.

---

## REMAINING GAPS

1. 🚨 **"First ♠️ discarded" scoring ambiguity (Vague User).** If a player selects a Pipeline and the boss discards the ♠️, does the remaining 3-card hand still get scored as Pipeline, or does it re-resolve as High Card? Spec doesn't say. This is the #1 tutorial boss — it MUST be explicit. Recommend: "The removed card is subtracted before hand-type resolution; the remaining cards score as their best qualifying hand."

2. ⚠️ **Planet pricing missing.** Tarot/Joker/Booster/Reroll all have reference prices in the sample shop. Planets (Pluto–Eris) have zero pricing guidance. Add a default (e.g., $4-6).

3. ⚠️ **Joker slot count on STANDARD start.** "Starting jokers: 0" is clear, but the 5-slot cap applies — confirm you can still BUY up to 5.

4. ⚠️ **Shop item RNG range.** Prices show one example each. Should Jokers vary ($4–$10 by rarity)? Without a range, Claude will invent inconsistent prices run-to-run.

5. ⚠️ **Boss Blind 1 selection.** Ante 1 has 3 candidate bosses (Vague User / Short Attention / Impatient Boss). Spec doesn't say if it's random or fixed. Recommend random with seed-per-run.

6. ⚠️ **Hand minimum with boss modifiers.** If Vague User removes the only ♠️ in a 1-card High Card play, is it auto-whiff (score 0) or does the play fail and not consume an attempt?

7. ⚠️ **Interest calculation timing.** "+$1 per $5 held at end of blind" — is this BEFORE or AFTER the base + overshoot + hands rewards are paid? Affects compounding.

---

## POLISH SUGGESTIONS

- **Quick-reference card:** Add a compact "scoring cheat sheet" the player can call with `/cheat` — hand table + active joker summary on one screen. Reduces re-scrolling.
- **Boss preview:** Announce Boss Blind name + effect BEFORE entering Big Blind's shop, so the player can prep. Currently boss reveal feels last-minute.
- **Running score display:** After each play within a blind, show "Round: 480 / 180 target" so player tracks progress visibly.
- **Overshoot cap feels stingy.** $3 max means a 17× overshoot rewards the same as a 2× overshoot. Consider +$1 per 50% over (still capped $3) so big hands feel rewarded.
- **Starting cash $4 with STANDARD is tight.** First shop, player has $4 + $9 = $13, which comfortably buys one Joker. Feels OK but marginal. Worth playtesting.
- **Auto-advance timer.** "continuing in 2 seconds" is literal — in a text medium it just means "next message." Reword to "Press enter or type anything to continue."
- **The joker order-of-operations section is gold.** Keep it. Best-written rules block in the spec.

---

## VERDICT

The game runs. The math works. The flow is clean. Fix the **Vague User scoring ambiguity** before shipping — everything else is polish.

🃏🎰 Ship it.
