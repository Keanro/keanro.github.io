---
date: 2024-03-15 12:00:00 +0300
title: Divesting Single Workspace from Enterprise Grid
subtitle: New York City, USA
description: >-
  Divesting AI company's workspace to protect their PI.
image: '/images/nyc.jpg'
tags: [Divestiture]
featured: false
---

An up-and-coming AI company upgraded their Slack plan to the Enterprise Grid and migrated multiple standalone workspaces into Grid. Soon after completing their migrations, the client realized that they had mistakenly migrated a workspace that they used to collaborate with external users. Now that these external users were Full Members in Grid, they had the potential to communicate with every employee in the client's organization and see their PI, like names and emails. Considering the client didn't want this, nor did they want to demote every external user to a guest, they asked to divest the single workspace from Grid.

To achieve this, I exported the entire Enterprise Grid and created a new folder that includes all DMs and MPDMs between the users in the target workspace and all channels found within that workspace. Using Python, I was able to programmatically copy the folders and verify which conversations needed to be moved. Once the new folder was created, I imported it into an existing and empty Business+ workspace. Now that the workspace had all imported data, it was time to remove the data and users from the Enterprise Grid.

After confirming the divestiture was a success, the client deleted the workspace they had previously migrated and then we used SCIM to mass-deactivate all the external users that were imported into their Enterprise Grid.

#### Lessons learned
- If you want to export a workspace's full history within Grid, you need to export the entire Grid -- more info below.
- Certain data, like DMs, lives on the Organization level. When exporting Grid, you will have to combine both org-level and workspace-level data to achieve your intended results. This can get a bit tricky when tracking what you've done so far and whether the data is correct.

#### Extra Info
Divesting from Grid is a bit trickier than exporting a Business+ workspace. When exporting data from Grid, you can go into the Enterprise Grid Admin dashboard and export the target workspace, but you can only do so in 180 day increments. To circumvent this, you can go into ✨ **any** ✨ workspace within Grid and export the full history via the settings page. Why any workspace? Who knows! When exporting a workspace in Grid this way -- as opposed to the Enterprise Grid Admin dashboard -- Slack automatically exports all the other workspaces as well. Just remember, if the Grid has a lot of information, the export will take a long time.

Exporting the workspace this way produces a folder that is significantly different than a basic Business+ export. Multi-workspace channels, DMs and MPDMs, and workspaces are now in the main directory. Each workspace folder acts like a Business+ export, but it's not as simple as taking that folder and importing into a new workspace.
```
├── multi-workspace-channel-1
│   ├── 2023-08-11.json
│   │── 2023-08-12.json
├── dm-1
├── dm-2
├── workspace-1
│   ├── channel-1
│   ├── channel-2
│   ├── users.json
│   ├── channels.json
├── workspace-2
└── org_users.json
```