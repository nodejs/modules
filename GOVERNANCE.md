# Modules Team

For the current list of Team members, see the project
[README.md](./README.md).

## Members

The [nodejs/modules](https://github.com/nodejs/modules) GitHub
repository is maintained by the Team and additional Members who are
added on an ongoing basis.

There are two types of Members:

### Active Members

* Invited to all meetings
* Can participate in [consensus seeking process](#consensus-seeking-process)
* Counted towards quorum in [Team Meetings](#team-meetings)
* Participates in voting

### Observers

* Invited to all meetings
* Can participate in [consensus seeking process](#consensus-seeking-process)
* Not counted towards quorum in [Team Meetings](#team-meetings)
* Cannot participate in voting

## Team Membership

Team Membership is not time-limited. There is no fixed size of the Team.

There is no specific set of requirements or qualifications for Team Membership beyond these rules.

Changes to Team membership should be proposed with an issue and labelled `modules-agenda`
to be included in the next [team meeting](#team-meetings). Decisions are made via the
[Consensus Seeking Process](#consensus-seeking-process). If there are not objections in the
issue new members may attend the meeting in which their membership is officially accepted.

If a Team member is unable to attend a meeting where a planned membership decision is being made,
then their consent is assumed.

New Members to the team are initially accepted as Observers.

Observers can request to be made Active Members following the above process.

Active Members requesting to be made Observers following the above process are automatically approved
by the Team and do not require consensus.

Any Member requesting to be removed from the group following the above process are automatically approved
by the Team and do not require consensus.

## Team Meetings

The Team meets bi-weekly on Zoom.us. A designated moderator
approved by the Team runs the meeting. Each meeting should be
published to YouTube.

Items are added to the Team agenda that are considered contentious or
are modifications of governance, contribution policy, Team membership,
or release process.

The intention of the agenda is not to approve or review all patches;
that should happen continuously on GitHub and be handled by the larger
group of Collaborators.

Any community member or contributor can ask that something be added to
the next meeting's agenda by logging a GitHub Issue. Any Collaborator,
Team member or the moderator can add the item to the agenda by adding
the ***modules-agenda*** tag to the issue.

Prior to each Team meeting the moderator will share the agenda with
members of the Team. Team members can add any items they like to the
agenda at the beginning of each meeting. The moderator and the Team
cannot veto or remove items.

The Team may invite persons or representatives from certain projects to
participate in a non-voting capacity.

Decisions in meetings are made via the [Consensus Seeking Process](#Consensus-Seeking-Process)
and require a quorum of 50% of Active Members.

The moderator is responsible for summarizing the discussion of each
agenda item and sends it as a pull request after the meeting.

## Consensus Seeking Process

The Team follows a
[Consensus Seeking](http://en.wikipedia.org/wiki/Consensus-seeking_decision-making)
decision-making model.

When an agenda item has appeared to reach a consensus, the moderator
will ask "Does anyone object?" as a final call for dissent from the
consensus.

If an agenda item cannot reach a consensus, a Team member can call for
the item to be decided by a vote or to table the issue to the next
meeting. In both cases the decision must be seconded by a majority of the Team
or else the discussion will continue. Simple majority wins. Only Active
Members participate in a vote.

## Pull Requests

This section details expectations for working group members involved in any pull
requests that relate to the group's [scope][modules-wg-purpose] and
[implementation work][modules-wg-plan].

The following expectations apply to all team members involved in a related pull
requests in the [Node.js core repository][nodejs-core] or any of the Modules
working group's repositories, including [this Repository][nodejs-modules] and
the [nodejs/ecmascript-modules][nodejs-ecmascript-modules] fork.

These expectations are intended to ensure that all concurrent efforts align
with the overall direction of the group for delivering a cohesive and predictable
user experience for ECMAScript, CommonJS, and other modules supported by Node.js.

Pull requests not included under the _special exemptions_ section below must
reach consensus in a meeting in order to be merged into the respective repository.

A pull request that is is unable to reach consensus cannot be merged into the
respective repository.

### Merging PRs into nodejs/node

The current triage process will flag any pull requests requiring the attention
of this working group's members where they will be expected to determine based on
the _special exemptions_ sections outlined below if explicit consensus is needed
in the following meeting, or if it is implied.

### Merging PRs into nodejs/ecmascript-modules

The group may decide that certain features first be implemented in
[nodejs/ecmascript-modules][nodejs-ecmascript-modules] before deciding if it
should be upstreamed.

Specific restrictions or allowances may be determined on a case basis,
adhering in principle to the same expectations outline below.

### Special Exemptions for Pull Requests

**Special Exemptions for nodejs/node**

Special exemption is made for pull requests seeking to make any of the following
changes to the [the Node.js core repository][nodejs-core]:

- Errata fixes.
- Editorial changes (*non-intent-altering* only).
- Semver Patch fixes.

**Special Exemptions for nodejs/ecmascript-modules and nodejs/modules**

Special exemption is made for pull requests seeking to make any of the following
changes to [the Modules repository][nodejs-modules] or [the nodejs/ecmascript-modules repository][nodejs-ecmascript-modules]:

- Errata fixes.
- Editorial changes.
- Meeting minutes.
- Updates to the team lists via the `ncu-sync` tool.
- Doc Fixes
- Tests
- Fixing Conflicts with a rebase

These pull requests may be merged without being presented at a meeting if a
reasonable time is given for review and there no dissent. The time period seen
as reasonable for review varies on a case by case basis as determined by the
author. A member may request a specific time period for review of such a pull
request not to exceed the next meeting date. If a time for review is requested,
members must wait for that time period to pass or review be completed prior to
that time.


<a id="developers-certificate-of-origin"></a>
## Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

* (a) The contribution was created in whole or in part by me and I
  have the right to submit it under the open source license
  indicated in the file; or

* (b) The contribution is based upon previous work that, to the best
  of my knowledge, is covered under an appropriate open source
  license and I have the right under that license to submit that
  work with modifications, whether created in whole or in part
  by me, under the same open source license (unless I am
  permitted to submit under a different license), as indicated
  in the file; or

* (c) The contribution was provided directly to me by some other
  person who certified (a), (b) or (c) and I have not modified
  it.

* (d) I understand and agree that this project and the contribution
  are public and that a record of the contribution (including all
  personal information I submit with it, including my sign-off) is
  maintained indefinitely and may be redistributed consistent with
  this project or the open source license(s) involved.


<!-- Links -->

[nodejs-modules]: https://github.com/nodejs/modules
[nodejs-core]: https://github.com/nodejs/node
[nodejs-ecmascript-modules]: https://github.com/nodejs/ecmascript-modules
[modules-wg-purpose]: ./README.md#purpose
[modules-wg-plan]: ./doc/plan-for-new-modules-implementation.md
