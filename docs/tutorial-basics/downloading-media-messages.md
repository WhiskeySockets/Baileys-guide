---
sidebar_position: 9
---

# Downloading Media Messages

If you want to save the media you received

```ts
import { writeFile } from 'fs/promises'
import { downloadMediaMessage } from '@whiskeysockets/baileys'

sock.ev.on('messages.upsert', async ({ messages }) => {
    const m = messages[0]

    if (!m.message) return // if there is no text or media message
    const messageType = Object.keys (m.message)[0]// get what type of message it is -- text, image, video
    // if the message is an image
    if (messageType === 'imageMessage') {
        // download the message
        const buffer = await downloadMediaMessage(
            m,
            'buffer',
            { },
            {
                logger,
                // pass this so that baileys can request a reupload of media
                // that has been deleted
                reuploadRequest: sock.updateMediaMessage
            }
        )
        // save to file
        await writeFile('./my-download.jpeg', buffer)
    }
}
```

**Note:** WhatsApp automatically removes old media from their servers. For the device to access said media -- a re-upload is required by another device that has it. This can be accomplished using:

```ts
const updatedMediaMsg = await sock.updateMediaMessage(msg);
```
