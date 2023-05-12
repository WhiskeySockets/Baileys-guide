---
sidebar_position: 8
---

# Update Presence

```ts
await sock.sendPresenceUpdate("available", id);
```

This lets the person/group with `id` know whether you're online, offline, typing etc.

`presence` can be one of the following:

```ts
type WAPresence =
  | "unavailable"
  | "available"
  | "composing"
  | "recording"
  | "paused";
```

The presence expires after about 10 seconds.

:::note

In the multi-device version of WhatsApp -- if a desktop client is active, WA doesn't send push notifications to the device. If you would like to receive said notifications -- mark your Baileys client offline using `sock.sendPresenceUpdate('unavailable')`

:::
