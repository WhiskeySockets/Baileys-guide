---
sidebar_position: 4
---

# Broadcast Lists & Stories

**Note:** messages currently cannot be sent to broadcast lists from the MD version.

- You can send messages to broadcast lists the same way you send messages to groups & individual chats.
- Right now, WA Web does not support creating broadcast lists, but you can still delete them.
- Broadcast IDs are in the format `12345678@broadcast`
- To query a broadcast list's recipients & name:
  ```ts
  const bList = await sock.getBroadcastListInfo("1234@broadcast");
  console.log(`list name: ${bList.name}, recps: ${bList.recipients}`);
  ```
