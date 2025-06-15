---
id: gameboardhub-movetoken
title: MoveToken SignalR Method
sidebar_label: MoveToken
---

# MoveToken (SignalR Hub Method)

This method allows moving a token on the D&D game board via SignalR.

---

## Hub URL

`/hubs/board`

---

## Method Name

`MoveToken`

---

## Request Parameters

The method accepts a single parameter — an object of type `MoveTokenRequestDto` with the following structure:

| Field        | Type   | Description                 | Example                              |
|--------------|---------|-----------------------------|------------------------------------|
| `gameBoardId`| GUID    | The unique ID of the game board | `e7b1a1b2-5d18-4f1a-82a1-123456789abc` |
| `userId`     | GUID    | The unique ID of the user       | `4e7c5d42-3241-4b21-bc7e-abcdef123456` |
| `tokenId`    | GUID    | The unique ID of the token      | `9f823fde-12bc-4dff-bf68-987654321fed` |
| `positionX`  | float   | The new X coordinate of the token | `10.5`                             |
| `positionY`  | float   | The new Y coordinate of the token | `5.75`                             |

---

## Return Value

This method does **not return** a value directly.

Instead, the server notifies clients about the result via SignalR events:

- If the move **fails**, the calling client receives an `"Error"` event with this payload:

```json
{
  "isSuccess": false,
  "errorMessage": "Description of the error"
}
```

- If the move **succeeds**, the calling client receives an `"MoveToken"` event with this payload:

```json
{
  "isSuccess": true,
  "value": {
    "positionX": 10.5,
    "positionY": 5.75
  },
  "errorMessage": null
}