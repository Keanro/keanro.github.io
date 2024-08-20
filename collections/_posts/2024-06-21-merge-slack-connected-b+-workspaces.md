---
date: 2024-06-21 12:00:00 +0300
title: Merge Slack Connected Business+ Workspaces
subtitle: Cliffs of Moher, Ireland
description: >-
  Merging two Slack Connected Business+ workspaces.
image: '/images/cliffs-of-moher.jpg'
tags: [Migration]
featured: true
---

What happens when one organization acquires another, and they both use Slack? Well, unless one organization is using Enterprise Grid and the other isn't, chances are high for a manual migration. For this project, a Digital Marketing Agency (DMA) acquired a Data Insights Firm (DIF) but wanted to migrate into the DIF's Slack workspace. The end goal was to have everyone from both organizations in one dedicated workspace with all data. 

#### Considerations for this migration
- The Digital Marketing Agency wanted to migrate into the acquired Data Insights Firm's workspace as the Data Insight Firm had more brand recognition and had more Slack usage/data
- Both organizations had already been working together via Slack Connect channels and DMs
  - Any Slack Connect channel owned by the Data Insights Firm that had external parties was not to be touched whatsoever
- As both of these organizations are focused on external clients, we needed to ensure minimal downtime/bother for their external clients

_I keep saying the word migration, but it's not really a migration where one workspace moves into another, but rather it's copying the data of one workspace and pasting it into another. This distinction is important for the following sections._

When migrating one workspace into another, the level of complexity is determined by how well, or poor, they are connected. The higher the connection, the higher the complexity. This complexity does not present itself via Slack Connect DMs, but rather normal channels and Slack Connect channels. Slack Connect DMs are constant no matter which organization you're viewing them through: it's the same set of users and the DM ID is the same. When looking at Slack Connect channels, the channel ID and member list are the same but the channel name and visibility could be different. For normal channels, understanding what to do if their name is already being used in the target workspace is also required.

To prepare for the migration, I had to export the list of channels between both workspaces and analyze each scenario: channels with the same name, channels with the same ID (Slack Connect), and channels that _could_ be similar based off their names. 

#### Channels with the same name, but not Slack Connect
**Solution:** appended `<companyName>migrated-` to every channel, with few exceptions

**Reason:** there were many paths I could take, such as merging channels if they both were public, deleting one channel if there wasn't any data, even converting the private channels to public so I could then merge. Ultimately, the client and I agreed that the work would be too great to verify each channel and its contents. We settled on renaming every migrated channel, with the exception of a handful that the client wanted merged.

#### Slack Connect channels shared only between Digital Marketing Agency and Data Insight Firm
**Solution:** migrated every channel with a temporary name, deleted the existing version in the Data Insight Firm, and then renamed the channel back to the original name

**Reason:** for channels owned by the Data Insight Firm, we could've left them as-is and manually added back everyone from the Digital Marketing Agency, but those added-back users would see their messages from their old workspace account. Yes, the data was still there, but it wasn't *technically theirs*. If they were to search for their own messages in those channels, it wouldn't appear. To get around this, we imported the channels as brand new and made sure that every relevant user from both organizations was added to the channel. Now, rather than it looking like a Slack Connect version of yourself posted a message in the past, it will show as if you did it with your new account. Once imported, the client deleted all the 'older' versions of the channel and renamed the 'newer' versions back to the original name.


#### Lessons learned
- When disconnecting one organization from another in the Slack Connect settings, it removes all channels shared between both organizations even if a third party owns the channel
- Early on in the project, verify with the client team what their Data Retention Policy is
- When preparing for the migration, **verify** that the channel creator is still in the channel membership list
- Add the migrator's Slack User ID to every archived channel's member list for easy archiving with Slack APIs