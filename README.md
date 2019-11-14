# Orthopneumovirus

An office space chatting platform which is not constantly interrupting people while still gets the job done

## Skeleton

#### Conversations

To category conversations:

1. **Question/Task:** One user needs to resolve it. Once resolved, it is no longer important/become general information. When a group of people is assigned for it, the rest of the group doesn't need to be notified if somebody resolved it in the first place.
2. **Survey:** A group of people *all* need to reply it. Typically with a deadline and have no need for real time notification.
3. **General information**

#### Notifications

The final goal is to reduce the frequency of notifications popup, while important tasks can still be consumed in a timely manner.

Can have an algorithm to control sending notification, rather than always send them immediately. Policies may contain:

* Notifications can be grouped. Notifications which already exist but haven't arrived the sending threshold will be sending together which the triggered notification. The group of notifications shall have an order based on priority.
* Survey notification may be sent 1/N time before the deadline, so it is more likely to be sent with some other notification so is not an individual interrupter.
* When a question/task is assigned to a group of people, first ping one user, if it is not resolved in a while, ping the second user, and so on. Next action waiting time can be calibrated by (1) the size of the group, (2) how many people have been pinged. Actions like "looking emoji" or new reply may increase the waiting time (as that means one person is fully aware of the situation, and she can always ping the 2nd person manually).
	* For new conversion in the same thread, the person is not notified if "read", and notified by the same policy she was originally pinged (so pinged immediately if she's the first person work on it. Original person who asked the questions is treated as another prioritized notification target (So two people will be involved in a conversation). If the first person is not replied and the 2nd person is pinged, first person goes to the end of the notification row (slightly different if the first person has ever been involved/has real action in the conversation) and the 2nd person will be notified immediately). 
* Direct conversation and question/task assigned to a single person should be notified immediately (should we?).
* Non-working hour means temporarily opt-out the notification group, and weight is re-calibrated when the person is back. Original sender should be notified if everybody is temporarily out-out.

#### Clear history

For this to work, it is very important the chats has a clear history, which means 

* Historical questions/tasks should all in status "resolved" or "passed" (original poster define resolved, assigned person define passed).
	* Individual personal should also be able to "opt-out" a question/task.
		* Then there's a third status: everybody is opt-out?
	* For people who's already involved in a question/task, she should have a clear period without getting notification for that question/task (may based on the last time she replied to the thread and extended according to that).
	* What about if somebody "will work on it later"? Ideally she can create a ticket and resolve the conversion, but not sure if in real world people are so lazy to do so.
	* What about if request sender no longer responsive, but assigned person is not sure if the task can be passed?
* Surveys should all in status "timeout".

It can be maintained in a corporate group because otherwise those tasks will always in top priority in notification group. 

#### Bot

Only relate to a question/task. Just a user to opt-out the notification throttling, and always consume the task immediately.

#### Special situations

> @here Lunch is ready

Can technically be a survey (which makes sense, just the "reply" is to go and get lunch) with immediate deadline, but do people really need to know it immediately?

> @here I don't know whom to contact, but here is the issue ...

This is typically sent to a huge group of people (the entire company). Should not be allowed, and instead treated as a question/task to the HR (but then all in a sudden, HR needs to have some semi-on call rotation).

> fire

Person A is pinged as a normal question/task. If she need person B for help, she can ping person/group B in reply, and the notification weight is decoupled.

#### Groups/Channels

Channel is a way to achieve old conversation is a more organized way. If we don't need old conversation (all gone after resolved), we don't need channel.

Group can define the scope of each conversation.

`@here` in a channel is to treat the people in a channel as a group.

So there's actually no need to distinguish group and channel. 

Imaging

> person-A: group-1, group-2
> person-B: group-2, group-3

* person-A talks in group-2: general information for both A and B
* person-A @ group-3 in whatever channel: group-3 people will be notified
* person-A @ group-2 in whatever channel: she is treated as sender, so both her and person-B will be notified with highest priority.

People can both post on the group they belong to or not belong to. Just if they are not belong to that group, they can only access the conversations they are involved (asked question, @ed in conversation).

* Open group: Everybody can join (so then access full content of that group channel + all other channels this group is @ed) as an "observer" (opt out notification). Management is needed for group "full member".
* Private group: Everybody is a full member (so this unified Slack's "team").
	* Can somebody leave a private group by herself (should be yes).

List of groups for a person is defined by open group / private group, so side bar group list and access right becomes the same thing.

Then basically we unified group/channel/team with just open and private groups.