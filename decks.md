Decks Specification
===================

## Decks

### attributes

#### id

**Type:** Non-negative Integer

**Unique:** Yes

**Required:** Yes

#### name

**Type:** String

**Unique:** No

**Required:** Yes

#### description

**Type:** String

**Required:** No

#### children

**Type:** List<Deck ids>

**Required:** No

Decks may be nested into another deck.
