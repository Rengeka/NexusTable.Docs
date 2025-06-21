---
id: gameboardhub-createtoken
title: CreateToken SignalR Method
sidebar_label: CreateToken
---

# CreateToken (SignalR Hub Method)

This method allows a client to create a new token on a specific game board via SignalR.

---

## Hub URL

`/hubs/board`

---

## Method Name

`CreateToken`

---

## Request Parameters

The method accepts a single parameter — an object of type `CreateTokenReqeustDto` with the following structure:

| Field        | Type  | Description                                | Example                                  |
|--------------|-------|--------------------------------------------|------------------------------------------|
| `GameBoardId`| GUID  | The unique ID of the game board where token is created | `e7b1a1b2-5d18-4f1a-82a1-123456789abc` |
| `Title`      | string| The title/name of the token                | `Knight`                                 |
| `PositionX`  | float | X-coordinate position of the token on the board | `100.5`                                  |
| `PositionY`  | float | Y-coordinate position of the token on the board | `200.75`                                 |

---

## Return Value

This method does **not return** a direct value.

Instead, the server sends responses via SignalR events:

- To **all clients in the board group**: a notification about the newly created token.
- To the **calling client**: a success or error notification about the token creation.

---

### On Success

All clients in the same board group receive a `CreateToken` event with this payload:

```json
{
  "isSuccess": true,
  "value": {
    "tokenId": "123e4567-e89b-12d3-a456-426614174000",
    "title": "Knight",
    "positionX": 100.5,
    "positionY": 200.75
  },
  "errorMessage": null
}

The calling client receives a `CreateTokenSuccess` event with this payload:

```json
{
  "isSuccess": true,
  "value": {
    "tokenId": "123e4567-e89b-12d3-a456-426614174000",
    "message": "Token successfully created"
  },
  "errorMessage": null
}
```

If the token creation fails, the calling client receives a `CreateTokenError` event with this payload:

```json
{
  "isSuccess": false,
  "value": null,
  "errorMessage": "Description of the failure reason"
}
```

## Notes

- The method uses the mediator pattern to handle the creation logic.
- On success, it notifies all clients in the board group about the new token (via CreateToken event).
- The caller additionally receives a dedicated success or error event.
- The group name is formed as "board:{GameBoardId}".