---
layout: default
title: Probability of finding Dakmor with GGT
parent: Mathematical Proofs
nav_order: 5
---

| **Chance to find Dakmor**         |Number of| draw triggers | available |
|:----------------------------------|:-------:|:-------------:|:---------:|
| **Percent of deck that is lands** |    1    |       2       |     3     |
| 26%                               |   43%   |      76%      |    92%    |
| 27%                               |   46%   |      79%      |    94%    |
| 28%                               |   50%   |      82%      |    95%    |
| 29%                               |   53%   |      84%      |    96%    |
| 31%                               |   56%   |      87%      |    97%    |
| 32%                               |   59%   |      89%      |    98%    |
| 33%                               |   62%   |      90%      |    98%    |


The purpose of this is not to lay out some new tech or grand reimagination but
rather to provide better estimates of things that are already known.

The previous sims were lacking some realistic details and were only simming
specific deck sizes. Here, we add in the consideration that when we use a draw
trigger from those available at the beginning, we are adding back a land into
the library after the next shuffle, and more. The big difference here, though,
is that we average across a range of deck sizes by using land count to library
size ratios instead of cherry picking specific deck sizes and specific land
counts.

A "sim" is an instance of dredge tutoring until the player runs out of draw
triggers to use, or finds dakmor (and is able to take it).

There are essentially two different versions of dredge tutoring that were
simmed. The first stipulated that there was never allowed to be 1 or fewer cards
in the library, in order to prevent a potential opponent from being able to
force a draw when the player was vulnerable. The second had no such rules,
employing loam dreding when the library was small, and counting the lands milled
to determine the ratio left in the library. The vast majority of the work I did
on this the past few days went into the latter. The difference between the two
when simmed was surprisingly minor: the first only fell behind by about 0.5-0.9
percent on each result.

The process for dredge tutoring in the [first version](https://github.com/TheGitrogServer/thegitrogserver.github.io/blob/main/docs/mathematical-proofs/ggt_normal.py)
is more or less as follows:
1. dredge 6, if we hit dakmor, great
	1. if the deck has 7 or fewer cards and we hit a shuffler, shuffle up and go
	back to start
	2. if there are 7 or fewer cards and we didn't hit a shuffler: (the success
	rateof hitting dakmor vs running out of draw triggers in this subsection is
	38%,with this subsection occurring about 1.6% of the time overall (excluding
	shuffling up and going to start scenarios))
	3. if we hit a land on the last dredge, we have to consume that draw by
	drawing one. Why not dredge loam if you have it? Well you could at 6 or 7
	cards left in library, but then after dredging 3, if you hit dakmor, dredge
	2, and hit a land, you are left at 1 card in library, which breaks the "1 or
	fewer" rule. The second version most certainly opts to dredge loam, and the
	success rate in the edge cases goes up accordingly.
	4. shuffle up and go to start
2. continue dredging. If we ran out of draw triggers, we failed.

In the [second version](https://github.com/TheGitrogServer/thegitrogserver.github.io/blob/main/docs/mathematical-proofs/ggt.py)
it gets a lot more complicated, with all kinds of edge cases when the library is
6 cards and under. I won't write out the process unless asked to, though.

The output of the first version looks like this:
```
This is an average from a library size of 65 to 85.
Total number of sims: 441,000,000. Total time: 538.37 secs.

Trigs:	  1     2     3     
0.26:    43.0  75.5  92.0
0.27:    46.2  78.7  93.6
0.28:    49.3  81.5  94.9
0.29:    52.6  84.2  95.9
0.31:    55.7  86.5  96.8
0.32:    59.1  88.7  97.5
0.33:    61.9  90.3  98.0
The first column is the (land count / library size) ratio.
I.e. for the max land ratio, 85 * 0.33 = 28 lands and 65 * 0.33 = 21 lands.
For the min land ratio, 85 * 0.26 = 22 lands and 65 * 0.26 = 17 lands.
```

The top row is the number of draw triggers we can generate before starting. The
first column is the ratio of land count in library to the library's size. This
is done so that we may average across different ratios, since we don't want to
average between say, 22 lands in 65 cards and 22 lands in 85 cards. In this
case, the ratios correspond to a range of land counts in 85 cards, i.e.
85 * 0.26 is 22 lands, 85 * 0.27 is 23 lands, ..., up to 28 lands. Equivalently,
for the low end of deck sizes at 65 cards, the ratios correspond to 17 lands up
to 21. The average across library sizes means the results for each library size
from 65, 66, ..., 84, up to 85 were added and divided by 21 (85 - 65 is 20, but
we add one as we include 85).

We sim many times so that the percentages converge to their real value.
