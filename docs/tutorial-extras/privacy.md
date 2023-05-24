---
sidebar_position: 3
---

# Privacy

## To get the privacy settings
  ```ts
  const privacySettings = await sock.fetchPrivacySettings(true);
  console.log("privacy settings: " + privacySettings);
  ```
## To update the LastSeen privacy
  ```ts
  const value = "all"; // 'contacts' | 'contact_blacklist' | 'none'
  await sock.updateLastSeenPrivacy(value);
  ```
## To update the Online privacy
  ```ts
  const value = "all"; // 'match_last_seen'
  await sock.updateOnlinePrivacy(value);
  ```
## To update the Profile Picture privacy
  ```ts
  const value = "all"; // 'contacts' | 'contact_blacklist' | 'none'
  await sock.updateProfilePicturePrivacy(value);
  ```
## To update the Status privacy
  ```ts
  const value = "all"; // 'contacts' | 'contact_blacklist' | 'none'
  await sock.updateStatusPrivacy(value);
  ```
## To update the Read Receipts privacy
  ```ts
  const value = "all"; // 'none'
  await sock.updateReadReceiptsPrivacy(value);
  ```
## To update the Groups Add privacy
  ```ts
  const value = "all"; // 'contacts' | 'contact_blacklist' | 'none'
  await sock.updateGroupsAddPrivacy(value);
  ```
## To update the Default Disappearing Mode
  ```ts
  const duration = 86400; // 604800 | 7776000 | 0
  await sock.updateDefaultDisappearingMode(duration);
  ```
