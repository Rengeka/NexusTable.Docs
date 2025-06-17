---
### id: gameboardhub-ondisconnectedasync

### title: OnDisconnectedAsync SignalR Method

### sidebar_label: OnDisconnectedAsync
---

# OnDisconnectedAsync

This method handles client disconnection from the SignalR hub and performs necessary cleanup and notifications.

---

## Hub URL

`/hubs/board`

---

## Method Description

When a client disconnects, this method:

- Checks if the client was connected to a board group by looking up `"BoardId"` in the connection context.
- If found, sends a `UserDisconnected` notification to all other clients in the same board group.
- Removes the disconnected client from the board group.
- Calls the base implementation to complete the disconnection process.

---

## Notifications Sent

### UserDisconnected Event

Sent to all clients in the board group (except the disconnected client) with the following payload:

```json
{
  "isSuccess": true,
  "value": {
    "userId": "00000000-0000-0000-0000-000000000000"
  },
  "errorMessage": null
}
```

## Notes

- The method ensures that all clients in the same board group are notified when a user disconnects.
- It properly removes the disconnected client from the SignalR group to prevent stale connections.
- This override is crucial for maintaining accurate real-time user presence in the game board.