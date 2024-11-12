---
date: 2024-09-14 12:00:00 +0300
title: Sales Engagement Platform Acquires Hyper-Slack-Users
subtitle: Hong Kong
description: >-
  Sales Engagement Platform acquires a Slack-first organization.
image: '/images/hong-kong.jpg'
tags: [Migration]
---

One of the largest Sales Engagement Platforms has acquired the industry leader in Conversational Marketing. For simplicity, I’ll refer to the acquired company as the ‘Child Company’ and the acquirer as the ‘Parent Company’ throughout this post. The Child Company has been using Slack extensively for internal processes and customer support. On the other hand, the Parent Company was relatively newer to Slack but understood the need to bring all users into the same environment. When merging organizations, it’s crucial to minimize the time spent in different Slack workspaces. The sooner everyone is working together, the faster the companies become one cohesive unit.

The Parent Company purchased Enterprise Grid, and my task was to migrate both companies’ workspaces into it. If you’ve read my previous posts, you know that this should be straightforward, but that’s not always the case. Both organizations came with their own set of hurdles: the Parent Company’s workspace already housed users from both organizations, and the Child Company’s workspace was primarily used by engineering teams to support infrastructure and clients.

#### Parent Company's Hurdles
Having users from both organizations already active in a single workspace seemed like a good move for company morale and the overall work experience. When a merger or acquisition occurs, many companies want to unite their employees under one environment as soon as possible. But from a migration perspective, it adds complexity -- especially when users already exist across both workspaces with different email addresses.

In migrating to Enterprise Grid, users can only be merged if they share the same email address across both workspaces. This means that once we migrated the Parent Company into Grid, any users who were already present in both workspaces had to have matching email addresses to ensure their data was properly merged.

The easiest way to address this was by using [SCIM](https://api.slack.com/admins/scim){:target="_blank"} to edit users’ email addresses. Through SCIM and Python, we updated 430+ user profiles in under a minute. We switched their email addresses from the Child Company domain to the Parent Company domain, since these users were already active in the Parent Company workspace with their new emails. Timing was key—this update had to happen close to migration to avoid causing confusion or concern among users in the Child Company workspace. As we all know, changes to user accounts can lead to unnecessary stress, so we kept communications limited.

#### Child Company's Hurdles
With most of the Child Company users already active in the Parent Company’s workspace, we were mainly concerned about the engineering teams who continued to rely on the Child Company’s workspace. The main issue was ensuring that support and service levels for their clients were not disrupted, as many of their critical apps and notification systems depended on Slack.

To address this, we conducted a detailed audit of all installed apps in the Child Company’s workspace to see which ones might be impacted during migration. The easiest way to know for custom apps was whether they had an admin scope. Apps with admin scopes would need to be re-installed at the Organization level after migrating to Enterprise Grid. Apps that didn’t rely on admin scopes would continue to work as expected.

If you've been doing this long enough, you've probably just asked yourself "don't admin scopes only work in Enterprise Grid?". You are correct! For those of y'all that don't know, the catch is that admin scopes are meant for specific administrative actions, but in non-Enterprise Grid workspaces the vast majority of these admin scopes don’t work. This posed a challenge, as the Child Company had installed many of their critical apps with the admin scope, which, as I mentioned, doesn’t work outside of Grid.

While I don’t fault the client for using admin scopes, I found it frustrating that Slack allows apps to be installed with these scopes even when they aren’t functional. After explaining the issue to the client, they chose not to remove the admin scope to avoid additional risk close to the migration date. Instead, we planned to quickly reinstall the apps post-migration on the Organization level and update their source code to use the newly generated Bot Token.

While the migration itself posed several challenges, post-migration presented its own set of issues...

#### Post-Migration Fallout
As with any large migration, there were some post-migration glitches: users had to log back in, some old messages appeared as new, and a few UI issues popped up that were easily fixed by clearing the cache. However, there was one issue that caught me completely off guard. After the Child Company’s workspace migrated into Enterprise Grid, users from the Parent Company began requesting access to the newly-migrated Child Company workspace.

Why? Now that is the million dollar question! If I knew why, you'd be looking at the world's #1 corporate psychologist. The Child Company workspace was mostly unused, aside from by the engineering teams. We couldn't say with 100% certainty *why* people were requesting access to the workspace. It’s likely that this was due to some expectation of functionality or available channels, but we didn’t get clear answers from the users, but the people reading this tend to work in IT... so y'all know the real answer. 

To quickly address the issue, we changed the workspace settings to "Open," allowing anyone to join without requiring admin approval. We considered restricting channel creation, but that would have blocked the critical business apps that we were originally worried about, so we opted for the safer route. This *situation* was a valuable reminder that collaboration strategy goes beyond simply merging workspaces—it’s about how users will collaborate effectively, especially post-acquisition.

#### Lessons learned
- **Focus on the task at hand**: It’s easy to get distracted by the urge to overhaul the client’s Slack usage, apps, and workspace settings, but doing so during a migration can introduce unnecessary risks. In this case, we focused solely on merging the two workspaces into Enterprise Grid and avoided complicating things further with additional changes that could have led to operational risks or service disruptions.
- **Plan for long-term collaboration**: This migration reminded us that it’s not just about moving users to a new platform, it’s about preparing them for how they’ll collaborate after the transition. The decision to set the Child Company workspace to "Open" may have made it easier to access, but it also opened the door for future administrative headaches. Will this encourage the creation of new channels in the Child Company’s workspace, will it dilute focus and reduce efficiency, will it overcomplicate corporate strategy, will it slow down the merging of the business?
  - To slightly counter/balance the above, our focus wasn't to create a strategy but to just migrate the workspace into Enterprise Grid. I have a soft spot for collaboration strategy.
- Always, always, **always** change the workspace visibility post-migration unless the client wants it left to the default "By Request".