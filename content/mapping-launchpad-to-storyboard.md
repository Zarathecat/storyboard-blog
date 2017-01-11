Title: Mapping Launchpad to StoryBoard
Date: 2016-12-08 12:00
Category: Info
Authors: Kendall Nelson, Zara Zaimeche, Jeremy Stanley, Ildiko Vancsa 

Mapping Launchpad to StoryBoard

One of the crucial parts of any transition is understanding the differences
between your old tools and new tools. It's also worth noting that StoryBoard 
was built with the knowledge of Launchpad's limitations, and so much of it was 
engineered to work around those issues. In an effort to help make this 
migration to StoryBoard easier, we have laid out some of the more widely used 
features and will discuss the differences between Launchpad and StoryBoard. 

For more information on StoryBoard specifically, check out the 
documentation [1], wiki[2], sandbox [3], and live UI[4].

## Filing Bugs & Planning Features

Launchpad uses a minimalist approach for bug reporting.
The submitter is only required to provide a summary description of the issue 
and then has the option to add other relevant information about the bug, tags, 
and other attachments before submitting. Bugs also have a status that can be 
modified throughout the process of triaging, confirming, and resolving. 
People are able to post comments on the bug as it goes through the process to 
communicate with other people watching the issue. 

Feature planning in Launchpad uses an equally minimalist process for 
registering a blueprint for a feature request. Blueprints are the 
classification for the tasks that focus on new features and changes. 
When creating a blueprint, the submitter need only provide a name (to be used 
for the url), the title of the feature request, and a summary description of 
the feature. They can also provide an assignee, drafter, approver, status 
other than new, and a proposed sprint that the blueprint is targeted for. 
After the blueprint has been created, it will be approved and work on 
implementing it can commence. In practice, the 'blueprints' feature is not
widely used in this way; instead, the community use 'specs' repositories
updated through code review, and tend to use 'blueprints' as hollow pointers
to these.

StoryBoard uses same process both for filing bugs and detailing feature 
requests; a user files a 'story', which can describe either a bug or a 
feature. A story has a 'description' field, in which a user can give details, 
'tags', to help classify the story, for use with filters, and comments, 
where users can add further information.

A story can also include multiple tasks, which will each be associated with a 
project (the projects can be the same or different for tasks in the same 
story). A task is a concrete item of work that should be undertaken in order 
to complete the story, so has a status (eg: 'in progress', 'review', 'merged').
 A story's status is derived from the statuses of its tasks; a story can be 
'active' (one or more tasks with the status 'todo', 'in progress' or 'review'),
 'merged' (all tasks 'merged') or 'invalid' (no tasks, or all tasks 'invalid'). 

Stories have unique IDs, as do tasks. If a user notes the story ID in a commit, 
comments will be left linking to gerrit whenever there are updates in the 
corresponding review. If a user notes a task ID in a commit, the corresponding 
task status will automatically update in StoryBoard when the patch first goes 
up for review, is merged, or is abandoned. You can read more details here [5] .
 Using 'stories' for both bug reports and feature requests protects users 
from long debates on whether something should be classed as an issue report or 
a feature request.


## Setting Priority 


Launchpad has the ability to set the importance for bugs and series goals for 
blueprints after they have been created, but only if you have the correct 
permissions to do so.

In contrast, in StoryBoard, a 'priority' setting is not applied directly to a 
task. Instead, a user can place a task in a worklist. The task then derives 
priority, *relative to that worklist*, from the task's position in that 
worklist.[6]. This means that a task can be present in multiple worklists, 
and have different priority relative to each one. Each of these worklists may 
be used by a different individual or team, and a user can subscribe to any 
worklist they are interested in. When viewing tasks, the user will then see 
tasks' priority relative to any subscribed worklists. Stories may also 
be placed in worklists, so a user may also prioritise stories.

This flexible priority provides multiple views, by allowing different people 
to state their priority for items, and users to see the priorities of those 
whose opinions are important to them. As well as visualizing more information, 
this approach saves time, preventing arguments over a single prioritized list 
during meetings or in other discussion on IRC. Different groups need not give 
the same task the same priority. Moreover, people do not need to worry
about their priority lists being overwritten by others; it is possible to set 
ACLs for worklists. This means a project team can have full control over its 
list of priorities. For more information on relative priority, you can see 
corresponding workitems and notes here [7].

Any projects relying on bug importance for classifying priority issues can 
still achieve a similar effect through consistent use of story tags (as well 
as the capability to create automatic worklists based on specific tag names).


## Milestones

Milestone targets can be defined for both blueprints and bugs in Launchpad 
if you have the correct permissions for the task-- a similar situation to 
setting priorities. In both types of tasks, the targeted milestones are set 
after the bug or bluperint is created. This is designed to tie release-tracking
to bugs and blueprints, but the Release Management team does not 
use Launchpad's milestone targeting, preferring more feature-rich 
release-note automation, coupled with leaving comments on bugs. The comments 
note the releases in which their fixes appear.


In StoryBoard, milestones are not yet developed in detail. The API has some 
support for milestones, as seen here [9], but milestones are not yet 
represented in the webclient, as we await feedback on what the most 
helpful visual representation would be. This is an area where we would love 
some guidance and help, so if milestones matter to you, please join our weekly 
meetings, or come over to #storyboard, to get involved [2]! 

## APIs

StoryBoard's design is API-first, as opposed to Launchpad, where some features 
in Launchpad's WebUI are not reflected in Launchpad's HTTP and SMTP APIs. 
StoryBoard is a 
Python-based service with a MySQL DB backend and a REST API; the web 
interface that people see at https://storyboard.openstack.org is a 
JavaScript-based API client and just one possible user interface. It is 
possible to create any UI you like to use with the API. You can also make 
custom scripts to get data which is not visible in the UI but present in the 
API. For example, jeblair has made a console interface, a la gertty [12], 
called boartty [13], so you can talk to StoryBoard through a terminal. 

In a separate blog 
post, we discussed the motivation for creating StoryBoard's API in detail [8], 
and we will cover the features of the API in detail in future blog posts, 
but the point is this: StoryBoard's API is ahead of its UI. The API was created 
to handle situations that revealed limitations in Launchpad. If you'd like a 
sneak-preview, check out our API documentation [10] or our DB-schema diagram 
[11] (that is circa July 2016, so is a little out-of-date, but conveys the general idea).

We hope this has been informative! In our next blog post, we'll focus more on
things that are new in StoryBoard, including more detail on our REST API, 
worklists and boards. In the meantime, feel free to ask any questions 
in #storyboard, and happy task-tracking!

[1] http://docs.openstack.org/infra/storyboard/

[2] https://wiki.openstack.org/wiki/StoryBoard

[3] https://storyboard-dev.openstack.org/

[4]  https://storyboard.openstack.org/

[5] http://docs.openstack.org/infra/manual/developers.html#development-workflow

[6] http://lists.openstack.org/pipermail/openstack-infra/2016-October/004791.html 

[7] https://storyboard.openstack.org/#!/story/329

[8] https://storyboard-blog.sotk.co.uk/why-storyboard-for-openstack.html 

[9] http://docs.openstack.org/infra/storyboard/webapi/v1.html#milestones

[10] http://docs.openstack.org/infra/storyboard/webapi/v1.html

[11] http://i.imgur.com/arN0wK3.png

[12] https://git.openstack.org/cgit/openstack/gertty/

[13] https://git.openstack.org/cgit/openstack/boartty/

