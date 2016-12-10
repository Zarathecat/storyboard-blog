Title: Why StoryBoard for OpenStack ?
Date: 2016-12-08 12:00
Category: Info
Author: ttx

The StoryBoard project was started a few years ago while we were looking
for (and failing to find) alternatives to Launchpad. Now that we are getting
closer to being able to roll out StoryBoard for task tracking across OpenStack
projects, it's not useless to come back to what motivated this project in the
first place. We are used to Launchpad after all, and it served us well. Why
change ?

## Workarounds

One thing to remember as we start discussing this is how much we worked
around Launchpad's limitations. It's difficult to use Launchpad to organize
our work, so we all used a variety of other tools to do that. I witnessed
this first-hand while doing release management: teams would use Trello boards,
etherpads or Google docs to maintain unwieldy lists of out-of-sync links to
RC1 bugs and reviews, because Launchpad "priorities" and milestone targeting
were not exactly cutting it. We got used to those workarounds to the point
where we don't see them anymore, but that doesn't mean we should forever
renounce having everything in one tool. Another issue is that every team
would use its own set of workarounds, making it hard to discover. For someone
working on multiple teams, those ad-hoc workarounds made it very difficult
to have visibility inside the processes of each team.

## Going beyond Launchpad's limits

We originally chose Launchpad because it's a great tool to track tasks across
a complex community. The concept of Bugs being project-neutral but having a
number of tasks affecting different projects is perfect: you can describe a
single problem you are trying to solve and then list all the tasks affecting
all the teams that will need to coordinate fully solve it. Most other trackers
maintain stronger isolation between "projects", which make this kind of
coordination harder. Launchpad worked well for us for a while. But as we
added more projects, the list of tasks grew, and we discovered that Launchpad
did not exactly handle that well for bugs with, say, more than 10 tasks. The UI
would constantly hit timeouts which would prevent you from editing the status
of any of those tasks. As a result, coordination on complex tasks moved to
other tracking solutions like etherpads or git documents. There is nothing
fundamentally complex in handling bugs with 50 tasks, though, and StoryBoard
does not have the same limitation. It replicates the Bug/Task concept (we
call bugs "stories" so that they can be used for more than just bugs) but
without the complex data model and interactions that plagued Launchpad's
performance when handling them.

## API-first

The main issue we have with Launchpad is its API. It doesn't reflect all
the features of the web UI, nor does it expose the full data model. You can
propose fixes to exhibit those (as I did for pieces of the blueprint data
model), but that doesn't change the fact that it's not API-first, so you are
always playing catch-up. The other thing is that the API also happens to be
barely usable. I mentioned above that the UI would frequently timeout when
manipulating bugs with a large number of tasks, but the corresponding API
calls would also timeout and fail whenever you manipulated large bugs. That
made most scripts very brittle and almost unusable in automation. The API
is important, because if there is a strong API you can work around any UI
limitation, and deeply automate your tasks. StoryBoard is API-first: the web
UI is actually a JavaScript static page making REST calls to an OpenStack-like
API server. You can actually do more with the API than with the UI, and the UI
is just one possible way to use the API.

## Not being stuck with UbuntuOne

Launchpad uses UbuntuOne OpenID for authentication, and you can't use
anything else. Since we required it for Launchpad, we ended up using it as
authentication for most of the upstream development properties:
review.openstack.org, wiki.openstack.org... But OpenStack also has its own
OpenID authentication (OpenStackID), used in your Foundation profile, which
you use for most of the downstream activities (registering to summit, voting
for Foundation individual members etc.). That creates a number of issues when
we need to connect those identities, like for example to know who signed the
CLA. A single identity would solve that. StoryBoard accepts any OpenID
provider: it uses UbuntuOne currently but can easily be switched to
OpenStackID authentication whenever we are ready.

## Why not push modifications upstream and/or run our own ?

Now the question is, couldn't we just improve Launchpad so that it suits our
needs, rather than reinvent the wheel by writing our own ? It is, after all,
developed under AGPLv3. We can (and some of us did) push a number of changes
to Launchpad. The issue is we are now hitting opinionated changes (like
wanting to use the Bugs backend for features, instead of the Blueprint
backend), or complex ones (like solving the whole data model issue that
triggers those damn timeouts). Without upstream support this is likely to
be an impossible task, and the bugs we filed over time did not exactly
generate much upstream excitement. We could fork it, modify it and run our
own... but Launchpad is notoriously difficult to run as a standalone
instance -- we happen to have a lot of ex-Launchpad devs in the OpenStack
community, and they all advised us against trying that path.

## So where does that leave us ?

We spent years evaluating alternatives. We created a set of requirements, and
no existing solution offered them all. An experimental OpenStack infra project,
called StoryBoard, was started based on those requirements. But it went slow
and we continued to consider alternatives. We came very close to implementing
one such alternative solution (Maniphest) but it was missing one of our key
requirements (an API-first design) and their developers were not really
interested in bending their product to support our (admittedly specific) use
case.

Fast-forward to today, where there is an organization outside of OpenStack
using StoryBoard, interested in supporting it in the future, and willing to
help with getting the must-have features we may still be missing. It feels
like this option has the most potential to become the one and unique task
tracker across OpenStack projects, so we are now reaching out to power users
to identify and cover all those must-haves before starting any large-scale
migration.
