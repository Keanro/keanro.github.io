---
date: 2024-05-24 12:00:00 +0300
title: Mattermost to Slack Migration
subtitle: Kuang Si Waterfall, Luang Prabang, Laos
description: >-
  Proof of Concept importing data from Mattermost to Slack.
image: '/images/luang-prabang.jpg'
tags: [Development, Python]
featured: false
---

[Mattermost](https://mattermost.com/) is an open-source collaboration tool that has similar functionality to Slack. While Mattermost isn't as big as Slack, there are organizations that use it for its self-hosting capability. 

As anyone within the IT department knows, organizations are constantly searching for ways to either save money by reducing costs or by increasing producitivty/efficiency (_and still reducing costs_). Oftentimes this means switching collaboration applications: Slack, Zoom, Teams, Mattermost, etc. This **Proof of Concept (POC)** demonstrates that migrating from Mattermost to Slack is possible. 

#### Considerations for this POC
- The channels, messages, users, and files are all real, but the messages and users have been randomized to protect client data. You can still see special characters such as @ : , ) ! -, but everything else has been replaced by X.
- Additionally, as this is a POC I did not go into a full-fledge import as that would require additional client support that I will detail below.

![Mattermost import](/images/mattermost.png){: width="600" height="450"}
*Randomized Mattermost data imported into Slack*

#### Creating the import file
Unfortunately, simply selecting a Mattermost export zip folder isn't enough to _just_ import it into Slack. The Mattermost export data structure is organized, but very different that Slack's required import data structure. The Mattermost data I received was broken into multiple CSV files: channels, private channels, DMs, and files. There wasn't a user file, so I had to create my own user file based off members found in each of the CSV files.

Going through the channels and private channels CSV files, I would create a new folder with the channel name and then create a JSON file for each date that channel had a message. Once all channel folders and data is created, I then create a dedicated file that the Slack Import Tool uses to reference the imported data. It looks something like the following: 
```
[
  {
  "id": channel_id, #can be random string starting with C
  "name": channel_name,
  "created": time.time()
  "creator": 'Slack_User_ID'
  "is_archived": False,
  "is_general": False,
  "members": member_list #list of member Slack IDs
  }
]
```

Once everything is created, I zip the folder up and import it! There are a few other steps not mentioned on this page, but the skills needed to do them are the exact same. 

#### Lessons learned
- Mattermost files are stored in the organization's file hosting service, such as AWS. Within the files CSV file, there are semi-broken URL references to the files itself, such as an uploaded screenshot or PDF. As I don't have access to the client file hosting service, nor do I know the beginning of their URL, I could not download/access the files. If we could figure out how to access the files using the link, we can either directly download the files OR replace them in the Slack messages so the Slack Import Tool can retrieve the files. As this is a POC, this is not confirmed.
- When thinking of a migration from Mattermost to Slack, we should really focus on the 80% of total data: channel messages and DMs. Items like emojis/reactions will not be migrated over, though we could upload emojis separately.

**Reach out to me if you're considering migrating away from Mattermost, or any other app for that _matter_!**
<!-- [Movie](https://github.com/Keanro/keanro.github.io/blob/336df357ae3954093da7d43c12e815b60d9f8891/images/schread/demo.mov) -->