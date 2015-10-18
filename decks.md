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

## REST API


### **Get a single deck**

```
GET /decks/:id
```

**Responses**

If the card doesn't exist, `404` status code is returned:

```json
HTTP/1.1 404 Not Found
Content-Length: 107
Content-Type: application/json; charset=utf-8
Date: Sun, 18 Oct 2015 02:44:37 GMT

{
    "developerMessage": "decks: no such deck of given id",
    "status": 404,
    "userMessage": "cannot find deck by id"
}
```

Otherwise, returns `200` status code with the deck contents:

```json
HTTP/1.1 200 OK
Content-Length: 85
Content-Type: application/json; charset=utf-8
Date: Sun, 18 Oct 2015 02:45:53 GMT

{
    "children": [
        3
    ],
    "description": "",
    "hasParent": true,
    "id": 2,
    "name": "MAT101",
    "parent": 1
}
```

### **Get many decks**

```
GET /decks
```

**Query Parameters**

Name  | Type         | Description
------|--------------|------------------------------------------------------------
decks | List<deckID> | A list of comma-separated deck ids (e.g. `?decks=1,2,3`)

**Responses**

If any of the deck ids does not exist, an error response is given:

```json
HTTP/1.1 404 Not Found
Content-Length: 107
Content-Type: application/json; charset=utf-8
Date: Sun, 18 Oct 2015 02:49:33 GMT

{
    "developerMessage": "decks: no such deck of given id",
    "status": 404,
    "userMessage": "cannot find deck by id"
}
```

If all the given deck ids exist, an array of the output identical to doing `GET /decks/:id` requests for each id is returned:

```json
HTTP/1.1 200 OK
Content-Length: 169
Content-Type: application/json; charset=utf-8
Date: Sun, 18 Oct 2015 02:46:56 GMT

[
    {
        "children": [
            3
        ],
        "description": "",
        "hasParent": true,
        "id": 2,
        "name": "MAT101",
        "parent": 1
    },
    {
        "children": [],
        "description": "",
        "hasParent": true,
        "id": 3,
        "name": "math",
        "parent": 2
    }
]
```

#### **Create a deck**

```
POST /decks
```

**JSON Input Request parameters**

Name        | Type                 | Description
------------|----------------------|------------------------------------------------------------
name        | Non-empty String     | Name of the deck **(required)**
description | String               | Description of the deck **(optional)**
parent      | Non-negative integer | Parent deck that the new deck belongs to **(required)**
