---
layout: default
title: Proof of Determinacy Addendum 1
parent: Mathematical Proofs
nav_order: 4
---

# Proof of Determinacy for the Gitrog Monster Combo - Addendum 1: Gaea's Blessing

When I wrote the original algorithm, I did not expect Gaea's Blessing to be a very meaningful card. Clearly it is not good enough to make titans obsolete since it is awkward when drawn, but it has turned out to be quite relevant in many decks regardless. It is particularly important when something like Anafenza is eliminating all creatures and denying a titan based loop. For this reason and other corner cases, I want to demonstrate that even Gaea's Blessing can deterministically generate draw triggers.

## Prerequisites
* The Gitrog Monster and a Putrid Imp are in play
* Dakmor Salvage is in hand
* Gaea's Blessing is in the deck
* Both the deck and graveyard each have an even number of cards
* The deck/graveyard has at least 5 lands to work with (not counting Dakmor Salvage)

## Algorithm
```
While you need more draw triggers:
- Dredge until you find Gaea's Blessing putting all draw triggers on the stack
- If no triggers were generated and deck has at least 5 lands:
- - Dredge until two draw triggers are generated
- - Draw two cards
- Resolve the shuffle
- Discard an even number of lands to ensure deck + graveyard has at least 5 lands, putting all draw triggers on the stack
```

## Proof

Definitions
```
  L => Land
  K => Kozilek (or Ulamog, only 1 in the deck)
  X => some non-L non-K card
  N => number of X in deck
  T => 'draw a card' trigger (from dredging a land)
```

Proving this combo to be determinate requires
1. Each state mills G such that the combo can continue
2. Each state increases T, reduces X, or leads unidirectionally towards a state which does so

(Reducing X is considered progress because they are not reintroduced and it is thereby bounded by the number in the deck)

Ensuring continuation
```
1. If the deck is even and you mill until G, you will always mill G.
2. If you only remove cards from the deck in pairs, the deck will remain even.
3. Adherance to the above will ensure the combo can continue
```
Ensuring progress:
```
1. If any L are milled before G or milled with G, T increases, so progress is 
   certain in these cases. What remains is the situation where all L are after G.
2. If there are at least 5 L left in the deck, two T can be generated and resolved
   regardless of their order or proximity to the end of deck.
    2.1. for example, if they are all at the end of the deck: `[XL LL LL] mill XL,
	   mill LL, draw LL
    2.2. for example, if they are not continuous: [XL XX LL XX LX LX] mill XL,
	   mill XX, mill LL, draw LX
3. When the two T are resolved, the only possible draws are: XX, XL, LL
    3.1 If XX is drawn, number of nonlands is decreased
    3.2 If LL or XL were drawn, T has not increased but the state of the deck is
	   different, lets say it has changed from State-A to State-B
        3.2.1 After the shuffle, LL or XL can be discarded to increase T
        3.2.2 Finally, even if the State-B iteration fails to increase T, it will inevitably return to State-A with the now higher T (basically you consider this as two parts of a single cycle)
4. No other possibilities exist, so progress is certain.
```

## Note about even and odd decks:
Since you do not want to draw Gaea's Blessing, it is never safe to draw a card until Gaea's Blessing's shuffle trigger is on the stack. Therefore the ideal strategy is to dredge 2 until it flips. However, if the deck is odd, Gaea's Blessing could be the final card and therefore never flip. I would suggest that the best way to make an odd deck even is to dredge Life from the Loam once. This will only fizzle in the very rare case that Gaea's Blessing is the last card and Loam is the second or third last card.

Finally, whenever you are going to shuffle the deck, ensure the total is even by discarding a nonland if necessary. It could get a little weird if creatures are getting exiled, so be vigilant. I consider this part of the setup and will assume that everything has been fixed and balanced before starting the algorithm definition.

## Note about Tournament Shortcuts:

This is not available as a tournament shortcut because it has conditionals and branching logic. What that means is your opponents can force you to play it out if they feel like it. That is totally their right. The determinacy simply means that regardless of any conditions, no matter how unlucky you could possibly get, it will continue to gain triggers and their forcing you to play it out is fruitless unless they are trying to use up a tournament clock or something like that.

*This was originally [posted](https://old.reddit.com/r/CompetitiveEDH/comments/aylety/proof_of_determinacy_for_the_gitrog_monster_combo/) on Reddit by mazeTemporal.*
