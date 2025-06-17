
---
### id: gameboardhub-leaveboard

### title: LeaveBoard SignalR Method

### sidebar_label: LeaveBoard
---

# LeaveBoard (SignalR Hub Method)

This method allows a user to leave a specific game board group via SignalR.

---

## Hub URL

`/hubs/board`

---

## Method Name

`LeaveBoard`

---

## Request Parameters

The method accepts a single parameter — an object of type `LeaveBoardRequestDto` with the following structure:

| Field      | Type | Description                          | Example                                  |
|------------|------|------------------------------------|------------------------------------------|
| `BoardId`  | GUID | The unique ID of the game board to leave | `e7b1a1b2-5d18-4f1a-82a1-123456789abc` |
| `UserId`   | GUID | The unique ID of the user leaving the board | `4e7c5d42-3241-4b21-bc7e-abcdef123456` |

---

## Return Value

This method does **not return** a direct value.

Instead, the server sends responses via SignalR events:

- To all other clients in the same board group: a notification that a user has left.
- To the calling client: a success notification.

---

### On Success

All other clients in the board group receive a `UserLeftBoard` event with the following payload:

```json
{
  "isSuccess": true,
  "value": {
    "userId": "4e7c5d42-3241-4b21-bc7e-abcdef123456"
  },
  "errorMessage": null
}
```

The calling client receives a `LeaveBoardSuccess` event with this payload:

```json
{
  "isSuccess": true,
  "value": {
    "boardId": "e7b1a1b2-5d18-4f1a-82a1-123456789abc",
    "message": "Successfully left the board"
  },
  "errorMessage": null
}
```

---

## Notes

- The method removes the current connection from the SignalR group named `"board:{BoardId}"`.
- It notifies all remaining clients in the group that the user has left.
- The caller receives a confirmation message indicating successful leaving.
- This enables real-time updates about users leaving the game board.