---
date: 2024-11-07 12:00:00 +0300
title: Listening to Channel Conversion Events
subtitle: Amsterdam, Netherlands
description: >-
  Getting around Slack's lack of channel conversion events.
image: '/images/amsterdam.jpg'
tags: [Development, Community]
---

*This post was originally made in the Slack Champion Network workspace. To see the original post directly, [click here](https://slackchampionnetwork.slack.com/archives/C02C28Z3XA7/p1730963855327819){:target="_blank"} or if you're part of the Slack Community workspace [click here](https://community.slack.com/archives/C02C28Z3XA7/p1730963855327819){:target="_blank"}.*

As of November 07, 2024, Slack does not have an [API event](https://api.slack.com/apis/events-api){:target="_blank"} to notify us when a channel is converted from public to private, or vice-versa. To some organizations, it's imperative for them to know when it happens in order to prevent information from being shared across the org or to even prevent information from being siloed. While there might not be an event to listen to, there is a way to know when this happens.

As of now, there isn’t an event to listen to these changes, but there is a way to do it. The documentation was a bit hard to follow as it’s all over the place and incomplete. **Your bot must be in the channel to continue.**

Whenever a channel is converted to public or private, a message is posted into the channel notifying the users of the change. It is this [message that you have to listen](https://api.slack.com/events/message#facts){:target="_blank"} for. Using the **channels:history** and **groups:history** scopes, you’ll now start receiving all message notifications to your app ![blob-sweat](/images/channel-conversion/blob-sweat.gif){: width="25" height="25"} Lucky for us, we have [message subtypes](https://api.slack.com/events/message){:target="_blank"} which categorize what type of message is being sent.

All you need to do is [filter by subtype](https://tools.slack.dev/bolt-python/concepts/event-listening){:target="_blank"} (click the dropdown menu **Filtering on message subtypes**) by explicitly typing out the event type and the subtype you want filtered. In this scenario, you’d filter by **channel_convert_to_public** or **channel_convert_to_private**. You can have it in the same listener function within a list (Python).

Now that it’s filtered, you can continue with your business logic, such as notifying a central team or just tracking it. In the recording shared, I just post back in the channel with a simple message.

<!-- This way allows for continuation of text -->
<video width="100%" height="auto" controls>
  <source src="/images/channel-conversion/Listen to Channel Conversions.mov" type="video/mp4">
</video>

‎

This post is in response to a user's feedback in the Slack Community workspace, which can be found below. 

![Where I got the idea](/images/channel-conversion/thread-idea.png){: style="width: 300px; height: 450px; display: block; margin-left: auto; margin-right: auto;"}
*Blocked names for privacy reasons*