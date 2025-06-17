
---
### id: gameboardhub-joinboard

### title: JoinBoard SignalR Method

### sidebar_label: JoinBoard
---

# JoinBoard (SignalR Hub Method)

This method allows a user to join a specific game board group via SignalR.

---

## Hub URL

`/hubs/board`

---

## Method Name

`JoinBoard`

---

## Request Parameters

The method accepts a single parameter — an object of type `JoinBoardReqeustDto` with the following structure:

| Field      | Type | Description                          | Example                                  |
|------------|------|------------------------------------|------------------------------------------|
| `BoardId`  | GUID | The unique ID of the game board to join | `e7b1a1b2-5d18-4f1a-82a1-123456789abc` |
| `UserId`   | GUID | The unique ID of the user joining the board | `4e7c5d42-3241-4b21-bc7e-abcdef123456` |

---

## Return Value

This method does **not return** a direct value.

Instead, the server sends responses via SignalR events:

- To the calling client: a success notification.
- To all other clients in the same board group: a notification about the newly connected user.

---

### On Success

The calling client receives a `JoinBoardSuccess` event with the following payload:

```json
{
  "isSuccess": true,
  "value": {
    "message": "Successfully joined the board"
  },
  "errorMessage": null
}
```

All other clients in the board group receive a `UserConnected` event with this payload:

```json
{
  "userId": "4e7c5d42-3241-4b21-bc7e-abcdef123456"
}
```

---

## Notes

- The method adds the current connection to a SignalR group named `"board:{BoardId}"`.
- It notifies all other clients in the same group about the new user's connection, except the caller.
- The caller receives a confirmation message indicating successful joining.
- This enables real-time updates about users joining the game board.