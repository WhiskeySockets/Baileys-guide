---
sidebar_position: 3
---

# Handling Events

Baileys uses the EventEmitter syntax for events.
They're all nicely typed up, so you shouldn't have any issues with an Intellisense editor like VS Code.

The events are typed as mentioned here:

```ts
export type BaileysEventMap = {
  /** connection state has been updated -- WS closed, opened, connecting etc. */
  "connection.update": Partial<ConnectionState>;
  /** credentials updated -- some metadata, keys or something */
  "creds.update": Partial<AuthenticationCreds>;
  /** history sync, everything is reverse chronologically sorted */
  "messaging-history.set": {
    chats: Chat[];
    contacts: Contact[];
    messages: WAMessage[];
    isLatest: boolean;
  };
  /** upsert chats */
  "chats.upsert": Chat[];
  /** update the given chats */
  "chats.update": Partial<Chat>[];
  /** delete chats with given ID */
  "chats.delete": string[];
  /** presence of contact in a chat updated */
  "presence.update": {
    id: string;
    presences: { [participant: string]: PresenceData };
  };

  "contacts.upsert": Contact[];
  "contacts.update": Partial<Contact>[];

  "messages.delete": { keys: WAMessageKey[] } | { jid: string; all: true };
  "messages.update": WAMessageUpdate[];
  "messages.media-update": {
    key: WAMessageKey;
    media?: { ciphertext: Uint8Array; iv: Uint8Array };
    error?: Boom;
  }[];
  /**
   * add/update the given messages. If they were received while the connection was online,
   * the update will have type: "notify"
   *  */
  "messages.upsert": { messages: WAMessage[]; type: MessageUpsertType };
  /** message was reacted to. If reaction was removed -- then "reaction.text" will be falsey */
  "messages.reaction": { key: WAMessageKey; reaction: proto.IReaction }[];

  "message-receipt.update": MessageUserReceiptUpdate[];

  "groups.upsert": GroupMetadata[];
  "groups.update": Partial<GroupMetadata>[];
  /** apply an action to participants in a group */
  "group-participants.update": {
    id: string;
    participants: string[];
    action: ParticipantAction;
  };

  "blocklist.set": { blocklist: string[] };
  "blocklist.update": { blocklist: string[]; type: "add" | "remove" };
  /** Receive an update on a call, including when the call was received, rejected, accepted */
  call: WACallEvent[];
};
```

You can listen to these events like this:

```ts
const sock = makeWASocket();
sock.ev.on("messages.upsert", ({ messages }) => {
  console.log("got messages", messages);
});
```
