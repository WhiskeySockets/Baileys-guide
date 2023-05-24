---
sidebar_position: 1
---

# Misc

## To check if a given ID is on WhatsApp
    ``` ts
    const id = '123456'
    const [result] = await sock.onWhatsApp(id)
    if (result.exists) console.log (`${id} exists on WhatsApp, as jid: ${result.jid}`)
    ```
## To query chat history on a group or with someone
    TODO, if possible
## To get the status of some person
    ``` ts
    const status = await sock.fetchStatus("xyz@s.whatsapp.net")
    console.log("status: " + status)
    ```
## To change your profile status
    ``` ts
    const status = 'Hello World!'
    await sock.updateProfileStatus(status)
    ```
## To change your profile name
    ``` ts
    const name = 'My name'
    await sock.updateProfileName(name)
    ```
## To get the display picture of some person/group
    ``` ts
    // for low res picture
    const ppUrl = await sock.profilePictureUrl("xyz@g.us")
    console.log("download profile picture from: " + ppUrl)
    // for high res picture
    const ppUrl = await sock.profilePictureUrl("xyz@g.us", 'image')
    ```
## To change your display picture or a group's
    ``` ts
    const jid = '111234567890-1594482450@g.us' // can be your own too
    await sock.updateProfilePicture(jid, { url: './new-profile-picture.jpeg' })
    ```
## To remove your display picture or a group's
    ``` ts
    const jid = '111234567890-1594482450@g.us' // can be your own too
    await sock.removeProfilePicture(jid)
    ```
## To get someone's presence (if they're typing or online)
    ``` ts
    // the presence update is fetched and called here
    sock.ev.on('presence-update', json => console.log(json))
    // request updates for a chat
    await sock.presenceSubscribe("xyz@s.whatsapp.net") 
    ```
## To block or unblock user
    ``` ts
    await sock.updateBlockStatus("xyz@s.whatsapp.net", "block") // Block user
    await sock.updateBlockStatus("xyz@s.whatsapp.net", "unblock") // Unblock user
    ```
## To get a business profile, such as description or category
    ```ts
    const profile = await sock.getBusinessProfile("xyz@s.whatsapp.net")
    console.log("business description: " + profile.description + ", category: " + profile.category)
    ```
Of course, replace ``` xyz ``` with an actual ID. 