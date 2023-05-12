---
sidebar_position: 10
---

# Deleting Messages

``` ts
const jid = '1234@s.whatsapp.net' // can also be a group
const response = await sock.sendMessage(jid, { text: 'hello!' }) // send a message
// sends a message to delete the given message
// this deletes the message for everyone
await sock.sendMessage(jid, { delete: response.key })
```

**Note:** deleting for oneself is supported via `chatModify` (next section)