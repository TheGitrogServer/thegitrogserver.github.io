---
layout: default
title: Sorcery Loop
nav_order: 5
parent: Fundamental Loops
has_children: true
permalink: docs/fundamental-loops/sorcery-speed/
---

# Basic Sorcery-speed Loop

You can use this basic loop to cast any sorcery an arbitrary number of times. For example you can [loop Lotus Petal](../basic-loops/lotus-petal.md) to create an unbounded amount of mana.

## Requirements

* A discard outlet in play
* Empty stack, graveyard, and library
* Unbounded mana

## Procedure

1. Cast and resolve [*sorcery*]
1. Discard any land, holding priority on the draw trigger
1. Discard a titan, resolve shuffle trigger
1. Discard and dredge Dakmor, stacking the draw trigger below the shuffle and resolve the shuffle trigger only
1. Discard and dredge Dakmor, stacking the draw trigger below the shuffle again, but hold priority on the shuffle trigger
1. Resolve the shuffle trigger and all three draw triggers

You are now back at your initial board state after having cast a sorcery spell.
