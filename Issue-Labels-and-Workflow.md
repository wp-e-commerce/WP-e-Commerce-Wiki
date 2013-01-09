We use multiple labels to categorize and filter issue tickets. Labels can only be applied by members with `push` privilege. Maintaining labels is the best way to organize tickets and plan releases.

The labels we're using are following the same format: {prefix letter}:{label name}. Example: p:medium, s:delivered, t:bug.

# Prefix labels
* `t`: Ticket type, can be `t:bug`, `t:enhancement`, or `t:feature`.
* `s`: Ticket status, explained in Workflow section below.
* `p`: Ticket priority, can be `p:high`, `p:medium`, `p:low`.

# Workflow
Looking at a ticket status and the current assignee, we would know in a glance who is working (or is supposed to work) on a particular issue, and the current progress on that issue. Note that only members with `push` privilege can assign labels. So if you don't have `push` privilege and you want to communicate your working progress on a ticket, make sure you communicate that clearly in an issue comment and someone will assign the proper label for you.

## Unconfirmed tickets
An unconfirmed ticket needs feedback from the reporter. Usually this happens when a bug report can't be reproduced by the ticket reviewer. These tickets will be labeled `s:unconfirmed`.

## Invalid tickets
Tickets that are irrelevant, lack important details, or [go against our guidelines](Creating-issue-tickets) will be labeled `s:invalid`.

## "Won't fix" tickets
Tickets that we deem not important to implement, or doesn't affect that many users to matter will be labeled `s:wont-fix`. Don't take it personally if your ticket falls into this category. We just have much more pressing issues to attend to.

## Confirmed tickets
A confirmed ticket has been reviewed by a core team member and is ready to be given a milestone and assigned to a member to work on. These tickets will be labeled `s:confirmed`.

## Work in progress
When a ticket is assigned to a member, and is being worked on by that member, it should be labeled `s:in-progress`. If a ticket is assigned to a member for quite some time (say, a week) and it still doesn't have `s:in-progress` label, it's probably time to assign it to someone else.

## Delivered
When the assigned developer completed work on a ticket, and is ready to hand it in for code review, the ticket should be labeled `s:delivered`.

## Incomplete
When the delivered ticket has been reviewed and it has been deemed to require some more work, the ticket should be labeled `s:incomplete`, and the assigned developer should go back to work. It will be reviewed again when it is delivered again.

## Fixed and committed
When a ticket's solution is complete and approved to be committed, we can strip all of its `s` and `p` labels (except for the `s:confirmed` label), but retain the `t` label.