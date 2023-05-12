---
sidebar_position: 12
---

# Disappearing Messages

```ts
const jid = "1234@s.whatsapp.net"; // can also be a group
// turn on disappearing messages
await sock.sendMessage(
  jid,
  // this is 1 week in seconds -- how long you want messages to appear for
  { disappearingMessagesInChat: WA_DEFAULT_EPHEMERAL }
);
// will send as a disappearing message
await sock.sendMessage(
  jid,
  { text: "hello" },
  { ephemeralExpiration: WA_DEFAULT_EPHEMERAL }
);
// turn off disappearing messages
await sock.sendMessage(jid, { disappearingMessagesInChat: false });
```
