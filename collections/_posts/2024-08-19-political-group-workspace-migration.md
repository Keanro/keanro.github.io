---
date: 2024-08-19 12:00:00 +0300
title: The Hill Edition - Business+ to Enterprise Grid Migration
subtitle: Washington, D.C.
description: >-
  Migrating one of the USA's major political parties' Slack workspace into Grid.
image: '/images/washington-dc.jpg'
tags: [Migration]
featured: false
---

_This project is neither an endorsement nor opposition to any particular political party._

One major political party has been using a Business+ workspace for months and wanted to migrate into Enterprise Grid for additional security features. Considering how tumultuous politics is, especially during an election year, the client needed to migrate ASAP so they could start implementing better security such as automatic off-boarding, Data Loss Prevention, and Data Retention. Early on during the project, I learned that the client had three Slack environments:
1. Business+ workspace (this is the one to migrate)
2. Enterprise Grid (active and connected to Business+ workspace via Slack Connect)
3. Empty Enterprise Grid (where we were to migrate the workspace into)

It didn't initially make sense why they wanted to migrate the workspace into the empty Enterprise Grid, as the active Enterprise Grid was already connected via Slack Connect to the Business+ workspace. It wasn't until we did a deep dive between the Business+ and active Enterprise Grid that we realized _how_ they were being used. 

The client had separated their users into two groups: high-level and standard, for a lack of better words. The high-level users were people like COO's of their state political party and national advisors, whereas the standard users were field organizers, analysts, etc. The client used an Okta-managed Slack Connect channel between the two instances to allow certain users to communicate with people from the other group. Ultimately, they did not want full visibility into every single user as they didn't want someone such as a field organizer, who is focused exclusively in their local area, to be able to easily contact a regional officer in charge of managing a few states. 

With this, I now understood why the client wanted to migrate into an empty Enterprise Grid, and that was to continue this barrier. So the next question is: why wasn't the client using [Info Barriers](https://slack.com/help/articles/360056171734-Create-information-barriers-in-Slack)? Simply put, there would be too many unknowns during an election year and they were not comfortable with it.

With this information, I worked with the client to ensure SSO was configured and that all profile fields from the migrating workspace existed in the Enterprise Grid. Once that was set up, we migrated the workspace during the middle of the night and verified that everyone could log back in the following day. 

#### Lessons learned
- There is a default profile field for Phone that can only be updated by SCIM. If you wish to make it user-edit, you have to go into the Org Admin Dashboard > Settings > Organization settings > scroll down to Alternate Phone and enable it > edit Alternate Phone to Phone in Atlas (profile configuration). 
- When migrating a workspace into the Enterprise Grid, if the migrated workspace's name is already taken by an existing workspace in the Enterprise Grid, the migrated workspace name will have a 2 appended to the name. Example: washington -> washington2.