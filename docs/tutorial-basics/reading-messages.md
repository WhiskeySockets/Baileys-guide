---
sidebar_position: 7
---

# Reading Messages

A set of message keys must be explicitly marked read now.
In multi-device, you cannot mark an entire "chat" read as it were with Baileys Web.
This means you have to keep track of unread messages.

```ts
const key = {
  remoteJid: "1234-123@g.us",
  id: "AHASHH123123AHGA", // id of the message you want to read
  participant: "912121232@s.whatsapp.net", // the ID of the user that sent the  message (undefined for individual chats)
};
// pass to readMessages function
// can pass multiple keys to read multiple messages as well
await sock.readMessages([key]);
```

The message ID is the unique identifier of the message that you are marking as read.
On a `WAMessage`, the `messageID` can be accessed using `messageID = message.key.id`.
