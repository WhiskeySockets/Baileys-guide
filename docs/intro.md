---
sidebar_position: 1
---

# Tutorial Intro

Let's discover **Baileys in less than 5 minutes**.

## Getting Started

Get started by **creating a new nodejs project** with `package.json` file.

### What you'll need

- [Node.js](https://nodejs.org/en/download/) version 16.14 or above:
  - When installing Node.js, you are recommended to check all checkboxes related to dependencies.

## Install

To install the stable version:

```bash npm2yarn
npm install @whiskeysockets/baileys
```

To install the edge version:

```bash npm2yarn
npm install github:WhiskeySockets/Baileys
```

## Usage

To import the main function:

```typescript
import makeWASocket from "@whiskeysockets/baileys";
```

## Connecting

```typescript
import makeWASocket, { DisconnectReason } from "@whiskeysockets/baileys";
import { Boom } from "@hapi/boom";

async function connectToWhatsApp() {
  const sock = makeWASocket({
    // can provide additional config here
    printQRInTerminal: true,
  });
  sock.ev.on("connection.update", (update) => {
    const { connection, lastDisconnect } = update;
    if (connection === "close") {
      const shouldReconnect =
        (lastDisconnect.error as Boom)?.output?.statusCode !==
        DisconnectReason.loggedOut;
      console.log(
        "connection closed due to ",
        lastDisconnect.error,
        ", reconnecting ",
        shouldReconnect
      );
      // reconnect if not logged out
      if (shouldReconnect) {
        connectToWhatsApp();
      }
    } else if (connection === "open") {
      console.log("opened connection");
    }
  });
  sock.ev.on("messages.upsert", async (m) => {
    console.log(JSON.stringify(m, undefined, 2));

    console.log("replying to", m.messages[0].key.remoteJid);
    await sock.sendMessage(m.messages[0].key.remoteJid!, {
      text: "Hello there!",
    });
  });
}
// run in main file
connectToWhatsApp();
```

If the connection is successful, you will see a QR code printed on your terminal screen, scan it with WhatsApp on your phone and you'll be logged in!

:::note

Install `qrcode-terminal` using

```bash npm2yarn
npm install qrcode-terminal
```

to auto-print the QR to the terminal.

:::

:::note
The code to support the legacy version of WA Web (pre multi-device) has been removed in v5. Only the standard multi-device connection is now supported. This is done as WA seems to have completely dropped support for the legacy version.
:::
