---
sidebar_position: 4
---

# Implementing a Data Store

Baileys does not come with a defacto storage for chats, contacts, or messages. However, a simple in-memory implementation has been provided. The store listens for chat updates, new messages, message updates, etc., to always have an up-to-date version of the data.

It can be used as follows:

```ts
import makeWASocket, { makeInMemoryStore } from "@whiskeysockets/baileys";
// the store maintains the data of the WA connection in memory
// can be written out to a file & read from it
const store = makeInMemoryStore({});
// can be read from a file
store.readFromFile("./baileys_store.json");
// saves the state to a file every 10s
setInterval(() => {
  store.writeToFile("./baileys_store.json");
}, 10_000);

const sock = makeWASocket({});
// will listen from this socket
// the store can listen from a new socket once the current socket outlives its lifetime
store.bind(sock.ev);

sock.ev.on("chats.set", () => {
  // can use "store.chats" however you want, even after the socket dies out
  // "chats" => a KeyedDB instance
  console.log("got chats", store.chats.all());
});

sock.ev.on("contacts.set", () => {
  console.log("got contacts", Object.values(store.contacts));
});
```

The store also provides some simple functions such as `loadMessages` that utilize the store to speed up data retrieval.

:::note

I highly recommend building your own data store especially for MD connections, as storing someone's entire chat history in memory is a terrible waste of RAM.

:::
