---
sidebar_position: 11
---

# Modifying Chats

WA uses an encrypted form of communication to send chat/app updates. This has been implemented mostly and you can send the following updates:

- Archive a chat
  ```ts
  const lastMsgInChat = await getLastMessageInChat("123456@s.whatsapp.net"); // implement this on your end
  await sock.chatModify(
    { archive: true, lastMessages: [lastMsgInChat] },
    "123456@s.whatsapp.net"
  );
  ```
- Mute/unmute a chat
  ```ts
  // mute for 8 hours
  await sock.chatModify(
    { mute: 8 * 60 * 60 * 1000 },
    "123456@s.whatsapp.net",
    []
  );
  // unmute
  await sock.chatModify({ mute: null }, "123456@s.whatsapp.net", []);
  ```
- Mark a chat read/unread

  ```ts
  const lastMsgInChat = await getLastMessageInChat("123456@s.whatsapp.net"); // implement this on your end
  // mark it unread
  await sock.chatModify(
    { markRead: false, lastMessages: [lastMsgInChat] },
    "123456@s.whatsapp.net"
  );
  ```

- Delete a message for me

  ```ts
  await sock.chatModify(
    {
      clear: {
        messages: [
          { id: "ATWYHDNNWU81732J", fromMe: true, timestamp: "1654823909" },
        ],
      },
    },
    "123456@s.whatsapp.net",
    []
  );
  ```

- Delete a chat

  ```ts
  const lastMsgInChat = await getLastMessageInChat("123456@s.whatsapp.net"); // implement this on your end
  await sock.chatModify(
    {
      delete: true,
      lastMessages: [
        {
          key: lastMsgInChat.key,
          messageTimestamp: lastMsgInChat.messageTimestamp,
        },
      ],
    },
    "123456@s.whatsapp.net"
  );
  ```

- Pin/unpin a chat
  ```ts
  await sock.chatModify(
    {
      pin: true, // or `false` to unpin
    },
    "123456@s.whatsapp.net"
  );
  ```

**Note:** if you mess up one of your updates, WA can log you out of all your devices and you'll have to log in again.
