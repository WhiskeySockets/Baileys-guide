---
sidebar_position: 5
---

# Writing Custom Functionality

Baileys is written with custom functionality in mind. Instead of forking the project & re-writing the internals, you can simply write your own extensions.

First, enable the logging of unhandled messages from WhatsApp by setting:

```ts
const sock = makeWASocket({
  logger: P({ level: "debug" }),
});
```

This will enable you to see all sorts of messages WhatsApp sends in the console.

Some examples:

1. Functionality to track the battery percentage of your phone.
   You enable logging and you'll see a message about your battery pop up in the console:
   `{"level":10,"fromMe":false,"frame":{"tag":"ib","attrs":{"from":"@s.whatsapp.net"},"content":[{"tag":"edge_routing","attrs":{},"content":[{"tag":"routing_info","attrs":{},"content":{"type":"Buffer","data":[8,2,8,5]}}]}]},"msg":"communication"} `

   The "frame" is what the message received is, it has three components:

   - `tag` -- what this frame is about (eg. message will have "message")
   - `attrs` -- a string key-value pair with some metadata (contains ID of the message usually)
   - `content` -- the actual data (eg. a message node will have the actual message content in it)
   - read more about this format [here](https://github.com/WhiskeySockets/Baileys/tree/master/src/WABinary)

   You can register a callback for an event using the following:

   ```ts
   // for any message with tag 'edge_routing'
   sock.ws.on(`CB:edge_routing`, (node: BinaryNode) => {});
   // for any message with tag 'edge_routing' and id attribute = abcd
   sock.ws.on(`CB:edge_routing,id:abcd`, (node: BinaryNode) => {});
   // for any message with tag 'edge_routing', id attribute = abcd & first content node routing_info
   sock.ws.on(`CB:edge_routing,id:abcd,routing_info`, (node: BinaryNode) => {});
   ```

### Note

This library was originally a project for **CS-2362 at Ashoka University** and is in no way affiliated with WhatsApp. Use at your own discretion. Do not spam people with this.

Also, this repo is now licenced under GPL 3 since it uses [libsignal-node](https://git.questbook.io/backend/service-coderunner/-/merge_requests/1)
