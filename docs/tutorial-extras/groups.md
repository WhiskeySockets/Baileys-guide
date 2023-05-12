---
sidebar_position: 2
---

# Groups

- To create a group
  ```ts
  // title & participants
  const group = await sock.groupCreate("My Fab Group", [
    "1234@s.whatsapp.net",
    "4564@s.whatsapp.net",
  ]);
  console.log("created group with id: " + group.gid);
  sock.sendMessage(group.id, { text: "hello there" }); // say hello to everyone on the group
  ```
- To add/remove people to a group or demote/promote people
  ```ts
  // id & people to add to the group (will throw error if it fails)
  const response = await sock.groupParticipantsUpdate(
    "abcd-xyz@g.us",
    ["abcd@s.whatsapp.net", "efgh@s.whatsapp.net"],
    "add" // replace this parameter with "remove", "demote" or "promote"
  );
  ```
- To change the group's subject
  ```ts
  await sock.groupUpdateSubject("abcd-xyz@g.us", "New Subject!");
  ```
- To change the group's description
  ```ts
  await sock.groupUpdateDescription("abcd-xyz@g.us", "New Description!");
  ```
- To change group settings
  ```ts
  // only allow admins to send messages
  await sock.groupSettingUpdate("abcd-xyz@g.us", "announcement");
  // allow everyone to send messages
  await sock.groupSettingUpdate("abcd-xyz@g.us", "not_announcement");
  // allow everyone to modify the group's settings -- like display picture etc.
  await sock.groupSettingUpdate("abcd-xyz@g.us", "unlocked");
  // only allow admins to modify the group's settings
  await sock.groupSettingUpdate("abcd-xyz@g.us", "locked");
  ```
- To leave a group
  ```ts
  await sock.groupLeave("abcd-xyz@g.us"); // (will throw error if it fails)
  ```
- To get the invite code for a group
  ```ts
  const code = await sock.groupInviteCode("abcd-xyz@g.us");
  console.log("group code: " + code);
  ```
- To revoke the invite code in a group
  ```ts
  const code = await sock.groupRevokeInvite("abcd-xyz@g.us");
  console.log("New group code: " + code);
  ```
- To query the metadata of a group
  ```ts
  const metadata = await sock.groupMetadata("abcd-xyz@g.us");
  console.log(
    metadata.id +
      ", title: " +
      metadata.subject +
      ", description: " +
      metadata.desc
  );
  ```
- To join the group using the invitation code
  ```ts
  const response = await sock.groupAcceptInvite("xxx");
  console.log("joined to: " + response);
  ```
  Of course, replace `xxx` with invitation code.
- To get group info by invite code
  ```ts
  const response = await sock.groupGetInviteInfo("xxx");
  console.log("group information: " + response);
  ```
- To join the group using groupInviteMessage
  ```ts
  const response = await sock.groupAcceptInviteV4(
    "abcd@s.whatsapp.net",
    groupInviteMessage
  );
  console.log("joined to: " + response);
  ```
  Of course, replace `xxx` with invitation code.
