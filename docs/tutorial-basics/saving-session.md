---
sidebar_position: 2
---

# Saving & Restoring Sessions

You obviously don't want to keep scanning the QR code every time you want to connect.

So, you can load the credentials to log back in:

```ts
import makeWASocket, {
  BufferJSON,
  useMultiFileAuthState,
} from "@whiskeysockets/baileys";
import * as fs from "fs";

// utility function to help save the auth state in a single folder
// this function serves as a good guide to help write auth & key states for SQL/no-SQL databases, which I would recommend in any production grade system
const { state, saveCreds } = await useMultiFileAuthState("auth_info_baileys");
// will use the given state to connect
// so if valid credentials are available -- it'll connect without QR
const conn = makeWASocket({ auth: state });
// this will be called as soon as the credentials are updated
conn.ev.on("creds.update", saveCreds);
```

**Note:** When a message is received/sent, due to signal sessions needing updating, the auth keys (`authState.keys`) will update. Whenever that happens, you must save the updated keys (`authState.keys.set()` is called). Not doing so will prevent your messages from reaching the recipient & cause other unexpected consequences. The `useMultiFileAuthState` function automatically takes care of that, but for any other serious implementation -- you will need to be very careful with the key state management.

## Listening to Connection Updates

Baileys now fires the `connection.update` event to let you know something has updated in the connection. This data has the following structure:

```ts
type ConnectionState = {
  /** connection is now open, connecting or closed */
  connection: WAConnectionState;
  /** the error that caused the connection to close */
  lastDisconnect?: {
    error: Error;
    date: Date;
  };
  /** is this a new login */
  isNewLogin?: boolean;
  /** the current QR code */
  qr?: string;
  /** has the device received all pending notifications while it was offline */
  receivedPendingNotifications?: boolean;
};
```

**Note:** this also offers any updates to the QR
