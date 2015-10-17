Review Specification
====================

## Selecting cards for review

- Any and all cards in deck; and cards within their deck descendants.
- Any and all cards in a stash (akin to a temporary deck).

## Choosing the next card for review

For a given set of cards, the following algorithm will choose the next card for the user to review.

### Rank score

A bias factor, and other elements, such as the time the card was last review, will be added to a card's *raw score* to calculate the **rank score**. 

The (raw) score of a card is calculated as follows:

```
raw_score = (fails + 0.5) / (fails + successes + 1)
```

The **rank score** of a card is calculated as follows:

```
bias_factor = (fails + 1) / (fails + successes + times_reviewed / 3)
base = raw_score + 1

rank_score = raw_score * Log(time_last_reviewed * bias_factor + base) / Log(base)
```

The bias factor is chosen such that it:

- favour cards that are seen less frequently
- favour less successful cards
- penalizes more successful cards

**Rationale:**

- The rank score adds ordering for each card within an abritrary set, such that the card with the highest rank score would generally have the highest priority in being chosen for review.

-   Logarithmic growth is chosen over time decay since: `Log(age of universe in seconds)/log(2) < 60`
    
    That is, time decay places the score dangerously close to the [underflow level](https://en.wikipedia.org/wiki/Arithmetic_underflow) of a floating-point system.

### Decide the method for choosing the next card

Within a set of cards, we may select a subset of these cards that are *likely* to be the next card to be reviewed. This sub-selection may also be ordered in a certain way. 

1. **Only new cards:** Probability of 15%

2. **Cards reviewed for more than N hours ago:** Probability of 30%

3. **Top N least recently reviewed cards:** Probability of 55%

If there are no new cards, then the remaining methods are chosen with their probabilities scaled.

If there are no cards that have been reviewed for more than *N* hours ago (i.e. all cards are being reviewed within *N* hours), then the remaining methods are chosen with their probabilities scaled.

**Rationale:**

Add a degree of [randomness](https://en.wikipedia.org/wiki/Randomized_algorithm) for choosing the next card.

#### Only new cards

From a set of cards, select only new cards (i.e. cards that have never been reviewed), and randomly pick a new card.

#### Cards reviewed for more than N hours ago

From a set of cards, select cards reviewed for more than *N* hours ago; where *N* is a non-negative integer. Generally `N >= 3` is preferred, so that a card has a chance of being seen every N hours.

The user may pick a reasonable value of N for his/her preference.

**Use case:**

Alice reviews card A, and card B is her next card to be reviewed. The application doesn't measure how long it takes for Alice to review card B. After N hours, Alice decides to finish reviewing card B (or skip it). Card A is now 'old' enough to be reviewed and has a chance to be picked as the next card to be reviewed. The chance of card A to be picked diminishes when there is a large selection of cards that Alice wants to review.

**Choosing the next card:**

1. **Random card:** Probability of 65%
2. **Highest rank score:** Probability of 35%

#### Top N least recently reviewed cards

From a set of cards, order cards from least recently reviewed to most recently reviewed. Select the top *N* cards, where the size of *N* is the purgatory region size.

**Calculating the purgatory region size:**

```
number_of_cards := number of cards in set.

purgatory_size = ceil(0.2 * number_of_cards)
```

**Choosing the next card:**

1. **Random card:** Probability of 25%
2. **Highest rank score:** Probability of 65%
3. **Oldest card:** Probability of 10%

**Rationale:**

The purgatory region is sized and named so that cards with high recall rates (i.e. have low rank/raw score) usually stay within this region for quite a while. Nonetheless, any card within this region will have a chance of being reviewed.

## Reviewing a card

The user may decide various options on how they score their performance on a card:

**Fail**

- `fails = fails + 1`
- `success = success`

**Hard**

- `fails = fails + 4`
- `success = success`

**Success (good)**

- `fails = fails`
- `success = success + 1`

**Easy**

- `fails = fails`
- `success = success + 3`

**Skip**

User may decide to skip the card; leaving the card's performance (raw) score unaffected. This sets the card's `time_last_reviewed` to be now.

**Forgot**

Reset the card's success/fails as follows:

- `fails = 2` (minor boost in the card's raw score w.r.t a card that was newly created)
- `success = 0`

**Reset**

Reset the card's success/fails as follows:

- `fails = 0`
- `success = 0`


Unresolved questions
====================

- Can the purgatory region size be resized with respect to the 'average' raw card score of a set of cards?
- Are the chosen probabilities good enough?

RFCs
====

- For "*Cards reviewed for more than N hours ago*"" method, discard M cards with the lowest scores, and randomly pick a card.

