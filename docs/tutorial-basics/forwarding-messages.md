---
sidebar_position: 6
---

# Forwarding Messages

```ts
const msg = getMessageFromStore("455@s.whatsapp.net", "HSJHJWH7323HSJSJ"); // implement this on your end
await sock.sendMessage("1234@s.whatsapp.net", { forward: msg }); // WA forward the message!
```
