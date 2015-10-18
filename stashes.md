Stashes Specification
=====================

Stashes allow another dimension of organizing cards for review. A stash is akin to a 'temporary' deck.

**Use cases:**

- "blitz/cram" reviews for a set of cards
- Reviewing cross-cutting concepts. *Example:** Partial-differential equations for a physics course.

## Stashes

### attributes

#### id

- **Type:** Non-negative Integer
- **Unique:** Yes
- **Required:** Yes

#### name

- **Type:** String
- **Unique:** No
- **Required:** Yes

The name of the stash shall be non-empty.

#### description

- **Type:** String
- **Required:** No

#### cards

- **Type:** `List<Card ids>`
- **Required:** No

List of cards within this stash.
