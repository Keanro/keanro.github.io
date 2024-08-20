---
date: 2024-04-19 12:00:00 +0300
title: Hands-Off Merging of Two Workspaces
subtitle: Philadelphia, USA
description: >-
  Advisory support role helping client merge two Business+ workspaces into one.
image: '/images/philly.jpg'
tags: [Migration, Project_Mgmt]
featured: false
---

_This project was purely in a supervisory capacity, meaning I led/taught the IT staff on how to do the workspace imports themselves._

The client company had acquired two other companies and was in the process of merging all systems between them. As both the client company and the acquired companies were using Slack already, the client wanted to merge all three workspaces into a single workspace. Upgrading to Enterprise Grid would allow for an easier migration into a single, dedicated environment but the cost was prohibitive. The client had a dedicated team of IT staff that could do the work, but the IT staff was not familiar with the requirements on importing a Slack workspace especially with Slack Connect channels. 

**The two biggest tasks for this project were**
1. Teaching the IT staff what the Slack export folder structure means and how to manipulate it into an importable file
2. Analyzing the combined exports to flag possible errors, such as duplicate channel names

#### Slack export folder structure
When exporting from a non-Enterprise Grid plan, the folder structure is simple but how it works can be confusing. Each channel, DM, MPDM, and file conversation have their own folder and within each folder is a dated JSON containing that day's conversation. There are also a few JSON files, such as `users.json` and `integrations_logs.json`. Most of these JSONs hold the key to the migration: every user found in a channel's daily JSON only shows their Slack User ID, but the `users.json` maps that ID to their profile. The same principle applies to public channels: every channel found in the `channels.json` file has a dedicated folder matching the channel name, but the import process looks at the channel ID found within the JSON.

Teaching this concept to the IT staff went smoothly, as they quickly understood the relation between the JSON files and the folders themselves. The IT staff also quickly noted how changing the value for one item could cause the import tool to skip that item if they didn't match it back to the relevant JSON.

#### Analyzing the combined exports
Like any migration, there is a chance to import channels that have the same name. For channels that are **not** Slack Connect, this leaves us with the possibility of either merging the channels if they're both public -- such as #dogs -- or renaming one of them and importing as a separate channel. For channels that **are** Slack Connect, I advised the team to import with a new channel -- such as #new-channelName. 

The reason for importing the Slack Connect channels as new, assuming that the only organizations in those Slack Connect channels are the client and acquired organizations, is that this is the only way to ensure that all data within the Slack Connect channel is associated to the user's new profile in the dedicated single workspace. If we decided instead to not import the Slack Connect channels and just add everyone's new Slack profile into the channel, they could see their old messages but that data is still tied to their old organization. Simply put, if they were to search for their messages in that channel nothing would show up because it's a different Slack account in a different Slack workspace. 

#### Lessons learned
- While the IT staff were exceptional, there are always details and obscurities that they just won't know. It's up to me to be as explicit as possible when explaining these concepts without overloading them with information and technicalities. 
- Organizations that prefer to do the migration themselves are also significantly more hands-on with end-user concerns. In essence, the team takes on the bulk of the work.