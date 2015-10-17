Cards Specification
===================

## Cards

### attributes

#### id

**Type:** Non-negative Integer

**Unique:** Yes

**Required:** Yes

#### title

**Type:** String

**Unique:** No

**Required:** Yes

The title of the card shall be non-empty.

#### front

**Type:** String

**Required:** No

This is the contents for the front-side of the card. The user may put a question/hint for them to recall the answer to.

During review, this side is shown first.

#### back

**Type:** String

**Required:** No

This is the contents for the back-side of the card. The user may put an answer to the front-side of the card.

During review, this side is, at first, hidden.

#### description

**Type:** String

**Required:** No

#### deck

**Type:** Non-negative Integer

**Unique:** No

**Required:** Yes

This attribute is the deck that this card belongs to.

#### time_last_reviewed

**Type:** Unix timestamp (integer)

**Default value:** The same timestamp when the card was created.

#### times_reviewed

**Type:** Non-negative Integer

**Unique:** No

**Default value:** 0

This attribute is the number of times this card has been reviewed.

#### successes 

**Type:** Non-negative Integer

**Unique:** No

**Default value:** 0

This attribute is the atomic variable that measures how well the user knows this card; this is part of a score equation that measures this card's performance.

#### fails 

**Type:** Non-negative Integer

**Unique:** No

**Default value:** 0

This attribute is the atomic variable that measures that the user does not know this card; this is part of a score equation that measures this card's performance.


## Card Performance

The card's performance is measured using binary votes (successes and fails) that are akin to Reddit upvotes/downvotes.


The (raw) score of a card is calculated as follows:

```
raw_score = (fails + 0.5) / (fails + successes + 1)
```

Note that this is considered to be the **raw score** of the card. A bias factor, and other elements, such as the time the card was last review, will be added to this *raw score* to calculate the **rank score**. The rank score adds ordering for each card, such that the card with the highest rank score would generally have the highest priority in being chosen for review.

The derived score equation is based on the following references:

- http://planspace.org/2014/08/17/how-to-sort-by-average-rating/
- [How to Count Thumb-Ups and Thumb-Downs: User-Rating based Ranking of Items from an Axiomatic Perspective](http://www.dcs.bbk.ac.uk/~dell/publications/dellzhang_ictir2011.pdf) by D Zhang, R Mao, H Li, and J Mao.


