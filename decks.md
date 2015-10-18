Decks Specification
===================

## Attributes of a deck

### id

- **Type:** Non-negative Integer
- **Unique:** Yes
- **Required:** Yes

### name

- **Type:** String
- **Unique:** No
- **Required:** Yes

### description

- **Type:** String
- **Required:** No

### children

- **Type:** `List<Deck ids>`
- **Required:** No

Decks may be nested into another deck.

## Root deck

The application may have a root deck where any and all decks are a descendant of. The user may not be able to delete the root deck.
