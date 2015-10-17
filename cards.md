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


