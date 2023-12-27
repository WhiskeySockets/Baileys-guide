---
sidebar_position: 5
---

# Sending Messages

**Send all types of messages with a single function:**

### Non-Media Messages

```ts
import { MessageType, MessageOptions, Mimetype } from "@whiskeysockets/baileys";

const id = "abcd@s.whatsapp.net"; // the WhatsApp ID
// send a simple text!
const sentMsg = await sock.sendMessage(id, { text: "oh hello there" });
// send a reply messagge
const sentMsg = await sock.sendMessage(
  id,
  { text: "oh hello there" },
  { quoted: message }
);
// send a mentions message
const sentMsg = await sock.sendMessage(id, {
  text: "@12345678901",
  mentions: ["12345678901@s.whatsapp.net"],
});
// send a location!
const sentMsg = await sock.sendMessage(id, {
  location: { degreesLatitude: 24.121231, degreesLongitude: 55.1121221 },
});
// send a contact!
const vcard =
  "BEGIN:VCARD\n" + // metadata of the contact card
  "VERSION:3.0\n" +
  "FN:Jeff Singh\n" + // full name
  "ORG:Ashoka Uni;\n" + // the organization of the contact
  "TEL;type=CELL;type=VOICE;waid=911234567890:+91 12345 67890\n" + // WhatsApp ID + phone number
  "END:VCARD";
const sentMsg = await sock.sendMessage(id, {
  contacts: {
    displayName: "Jeff",
    contacts: [{ vcard }],
  },
});

const reactionMessage = {
  react: {
    text: "💖", // use an empty string to remove the reaction
    key: message.key,
  },
};

const sendMsg = await sock.sendMessage(id, reactionMessage);
```

### Sending messages with link previews

1. By default, WA MD does not have link generation when sent from the web
2. Baileys has a function to generate the content for these link previews
3. To enable this function's usage, add `link-preview-js` as a dependency to your project with `yarn add link-preview-js`
4. Send a link:

```ts
// send a link
const sentMsg = await sock.sendMessage(id, {
  text: "Hi, this was sent using https://github.com/adiwajshing/baileys",
});
```

### Media Messages

Sending media (video, stickers, images) is easier & more efficient than ever.

- You can specify a buffer, a local url or even a remote url.
- When specifying a media url, Baileys never loads the entire buffer into memory; it even encrypts the media as a readable stream.

```ts
import { MessageType, MessageOptions, Mimetype } from '@whiskeysockets/baileys'
// Sending gifs
await sock.sendMessage(
    id,
    {
        video: fs.readFileSync("Media/ma_gif.mp4"),
        caption: "hello!",
        gifPlayback: true
    }
)

await sock.sendMessage(
    id,
    {
        video: "./Media/ma_gif.mp4",
        caption: "hello!",
        gifPlayback: true
    }
)

// send an audio file
await sock.sendMessage(
    id,
    { audio: { url: "./Media/audio.mp3" }, mimetype: 'audio/mp4' }
    { url: "Media/audio.mp3" }, // can send mp3, mp4, & ogg
)
```

### Notes

- `id` is the WhatsApp ID of the person or group you're sending the message to.
  - It must be in the format `[country code][phone number]@s.whatsapp.net`
    - Example for people: `+19999999999@s.whatsapp.net`.
    - For groups, it must be in the format `123456789-123345@g.us`.
  - For broadcast lists, it's `[timestamp of creation]@broadcast`.
  - For stories, the ID is `status@broadcast`.
- For media messages, the thumbnail can be generated automatically for images & stickers provided you add `jimp` or `sharp` as a dependency in your project using `yarn add jimp` or `yarn add sharp`. Thumbnails for videos can also be generated automatically, though, you need to have `ffmpeg` installed on your system.
- **MiscGenerationOptions**: some extra info about the message. It can have the following **optional** values:
  ```ts
  const info: MessageOptions = {
    quoted: quotedMessage, // the message you want to quote
    contextInfo: { forwardingScore: 2, isForwarded: true }, // some random context info (can show a forwarded message with this too)
    timestamp: Date(), // optional, if you want to manually set the timestamp of the message
    caption: "hello there!", // (for media messages) the caption to send with the media (cannot be sent with stickers though)
    jpegThumbnail:
      "23GD#4/==" /*  (for location & media messages) has to be a base 64 encoded JPEG if you want to send a custom thumb, 
                                  or set to null if you don't want to send a thumbnail.
                                  Do not enter this field if you want to automatically generate a thumb
                              */,
    mimetype:
      Mimetype.pdf /* (for media messages) specify the type of media (optional for all media types except documents),
                                  import {Mimetype} from '@whiskeysockets/baileys'
                              */,
    fileName: "somefile.pdf", // (for media messages) file name for the media
    /* will send audio messages as voice notes, if set to true */
    ptt: true,
    /** Should it send as a disappearing messages.
     * By default 'chat' -- which follows the setting of the chat */
    ephemeralExpiration: WA_DEFAULT_EPHEMERAL,
  };
  ```
