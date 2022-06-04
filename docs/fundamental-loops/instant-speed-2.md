---
layout: default
title: Instant Loop 2
nav_order: 4
parent: Fundamental Loops
---

# Basic Instant-speed Loop 1

You can use this basic loop to do things like [get unbounded mana from Dark Ritual](../basic-loops/dark-ritual.md), or cast Assassin’s Trophy to erase the board, or cast Ebony Charm an arbitrary amount of times, etc.

## Requirements

* A discard outlet in play
* Empty stack, graveyard, and library
* Unbounded mana

## Procedure

1. Discard any land, holding priority on the draw trigger
1. Discard a titan, resolve shuffle trigger
1. Discard and dredge Dakmor, stacking the draw trigger below the shuffle and resolve the shuffle trigger only
1. Discard and dredge Dakmor, stacking the draw trigger below the shuffle again, but hold priority on the shuffle trigger
1. Cast and resolve [*instant*]
1. Resolve the shuffle trigger and all three draw triggers

You are now back at your initial board state after having cast an instant spell.
