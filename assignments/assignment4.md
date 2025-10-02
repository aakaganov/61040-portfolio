# Assignment 4
## Problem statement 

**A problem domain** : Social media - I enjoy spending time on social media, I like how it has the potential to connect people and keep people in contact with each other. However I do not like it when it becomes too much of a distraction or time sink.

**A problem** : Social media is designed to keep a user on the app for as long as possible. This is good for the company, bad for the user. Many people still want to be able to access the app for content or connection with others, but they don't want the addiction or doom scrolling associated with the application. It's a very common problem for social media users to spend too much time on the phone, become addicted, or lose track of time. Such actions affect individuals' sleep, productivity, and stress.
**A stakeholder list** : 
1) User - The individual who uses social media too often. This is the user who would directly benefit from an app that limits social media use. This includes children who should only be given access to certain features of an app, students who need to focus on classwork but spend too much time on their phone, and adults in workplace settings where such distractions are unprofessional. This user would be the individual who uses this product.<br>
2) 2nd degree user - This is the individual who is affected by someone in their life who overuses social media. This is an individual who may encourage the user to find a solution, such as this product. These individuals can include friends/partners who want to spend more time with a user/may notice these problems a user has with social media, parents who want to fix or prevent addictions within their kid, and teachers who may notice focus and behavioral issues in different students with social media addictions. The 2nd degree user is the individual who benefits from another's use of our product. <br>
3) Companies - Companies creating the distracting apps, unlike users and 2nd degree users, benefit from an individual's excessive social media use. Part of the reason so many people have such issues is because social media is designed to be addicting. This stakeholder is negatively impacted by the product, as a result the solutions must keep in mind potential ways social media companies may hinder the success and effectiveness of the product.<br>

**Evidence and comparables**:
1) [Pros and Cons of Social Media](https://www.brownhealth.org/be-well/social-media-good-bad-and-ugly) - Pros and Cons of Social Media - Reports that 97% of teens (13–17) use at least one major platform, with teens averaging 9 hours/day and tweens 6 hours/day on social media. Shows the massive scale of use, highlighting how time loss and “doomscrolling” have become widespread issues.
2) [Social media’s impact on our mental health and tips to use it safely](https://health.ucdavis.edu/blog/cultivating-health/social-medias-impact-our-mental-health-and-tips-to-use-it-safely/2024/05) - Summarizes research connecting heavy use with anxiety, depression, and reduced well-being, while also offering strategies for safer use. Demonstrates that the harms extend beyond wasted time to real health consequences.
3) [Social Media Addiction Statistics](https://www.lanierlawfirm.com/social-media-addiction/statistics/)Explains how platforms are designed to be addictive, activating dopamine pathways similar to drugs. Reinforces that design choices drive compulsive use, making self-control alone insufficient.
4) [Addictive potential of social media, explained](https://med.stanford.edu/news/insights/2021/10/addictive-potential-of-social-media-explained.html): Social media is highly addictive, the way social media triggers dopamine pathways is very similar to drugs. Social media is designed to give contstant novelity and social reward to its users, keeping users hooked while also causing stress, anxiety, and learned helplessness.
5) [The impact of social media usage and lifestyle habits on academic achievement: Insights from a developing country context](https://www.sciencedirect.com/science/article/abs/pii/S0190740920311713#:~:text=An%20hour%20spent%20on%20social,romantic%20relationships%20impairs%20academic%20performance.): The odds of good academic performance are lowered by 24% by an hour spent on social media. The academic performance of high school and college students are impaired by social media, limiting time spent on such apps has a good chance of subsequently performance.  

 ## Application Pitch
 **Name**: LockIn<br>
 
 **A motivation**: Social media is a common addiction, with its features being designed to keep users on the app. LockIn gives people back the control over their apps, allowing them to still enjoy the benefits of social media, without losing hours to the app. <br>
 
 **Key features**: <br>
Scroll Blocker - Prevents users from entering scroll feeds (e.g., TikTok, Instagram Reels, YouTube Shorts). This prevents doomscrolling by blocking the distracting features of a social media application, while still allowing users to access other important parts of the app, such as messaging and long form videos.<br>
Accountability - This feature gives users the ability to share their screen time and app usage with selected friends, with the optional ability to have competitions for lowest screen time or alerts if someone deletes the app. This feature allows for an easy avenue for users to effectively receive social support. <br>
To-Do Unlock - Blocks social media until user completes tasks in a to do list, with each item completion being confirmed by either the user or a peer. Encourages productivity by removing the distraction until necessary tasks are completed<br>

## Concept design
**The concept specifications**
**Concept 1**:<br>
Purpose: Prevent a user from accessing endless scrolling feeds during certain times.

Principle: Users can only scroll in allowed apps outside blocked periods.

State:

A set of Users with:
blockedPeriods: Set<TimeInterval>
blockedApps: Set<App>
scrollingEnabled: Map<App, Boolean>

Actions:

setBlock(user: User, period: TimeInterval, apps: Set<App>)
requires: apps ≠ ∅
effect: adds period to blockedPeriods and apps to blockedApps

disableScroll(user: User, app: App)
requires: app ∈ blockedApps
effect: sets scrollingEnabled[app] = false

enableScroll(user: User, app: App)
requires: app ∈ blockedApps
effect: sets scrollingEnabled[app] = true

canScroll(user: User, app: App, time: Time): (ok: Boolean)
requires: app ∈ blockedApps
effect: returns false if time ∈ blockedPeriods, else true

**Concept 2**:<br>
Purpose: Track and compare screen time among peers.

Principle: Peer visibility encourages healthy app use.

State:

A set of Users with:
peers: Set<User>
screenTime: Number

Actions:

updateScreenTime(user: User, time: Number)
requires: time ≥ 0
effect: sets screenTime = time

shareScreenTime(user: User, peer: User)
requires: peer ∈ peers
effect: peer can view user’s screenTime

listPeersTimes(user: User): (list: Map<User, Number>)
requires: peers ≠ ∅
effect: returns all screenTimes of user’s peers

**Concept 3**:<br>
Purpose: Notify peers if a user stops sending screen time data (indicating they quit the app).

Principle: Transparency prevents silent opt-outs.

State:

A set of Users with:
lastUpdate: Timestamp
active: Boolean

Actions:

heartbeat(user: User, timestamp: Timestamp)
requires: timestamp > lastUpdate
effect: updates lastUpdate, sets active = true

markInactive(user: User)	
requires: lastUpdate older than threshold
effect: sets active = false

notifyPeers(user: User)
requires: active = false
effect: alerts all peers

**Concept 4**:<br>

Purpose: Require completion of all tasks before a user can access specified apps.

Principle: Productivity before entertainment.

State:

A set of Users with:
taskList: Set<Task>
completed: Set<Task>
gatedApps: Set<App>

Actions:

addTask(user: User, task: Task)
requires: task ∉ taskList ∪ completed
effect: adds task to taskList

markDone(user: User, task: Task)
requires: task ∈ taskList
effect: moves task from taskList to completed

allDone(user: User): (ok: Boolean)
requires: taskList ≠ ∅
effect: returns true if taskList ⊆ completed

unlockApps(user: User)
requires: allDone(user) = true
effect: removes apps from gatedApps

**Some essential synchronizations**:
sync enforceScrollBlock
when time ∈ ScrollBlock.blockedPeriods(user)
then for each app ∈ blockedApps → ScrollBlock.disableScroll(user, app)

sync updatePeerTimes
when Accountability.updateScreenTime(user, time)
then for each peer ∈ Accountability.peers → Accountability.shareScreenTime(user, peer)

sync detectQuit
when Presence.markInactive(user)
then Presence.notifyPeers(user)

sync unlockAfterTasks
when TaskGate.allDone(user): (ok = true)
then TaskGate.unlockApps(user)

**A brief note**:
Concept 1, Scroll Blocker,  stops users from endlessly scrolling in certain apps during set times. It’s the main way the app helps people control themselves. Concept 2, Accountability, adds a social layer—users can see each other’s screen time, so it encourages healthier habits. The users here are the same ones from the Scroll Blocker concept. Concept 3, Dropout Notification, makes sure people aren’t sneaking out of the system—it notices when someone stops reporting their screen time and lets their peers know, so everyone can trust the shared data. Concept 4, To-Do Unlock, makes users finish their tasks before they can use certain apps, again working with the same group of users as the other concepts.

All of these concepts work on their own, but together they fit around the same basic idea: the user. Scroll blocking, accountability, dropout notifications, and task gating can each do their thing separately, but when you put them together, they create a system that helps users manage their social media use, get support from peers, and stay productive. LockIn tackles the problem of social media addiction from a few angles without making any single part too complicated.

## UI Sketches

## User journey
The user picks up their phone in the morning, planning to check social media, but the app immediately blocks scrolling on certain platforms because it’s during a blocked time. A message pops up reminding them that scrolling will be available later and nudges them to finish any pending tasks first. They check their to-do list and mark off a few tasks, and only after everything is done do the blocked apps become usable. During the day, their friends can see updates on their screen time, which adds a sense of accountability and a little friendly competition. Later, the user considers deleting the app to use social media, but doesnt because their peers would be notified. By the end of the day, the user has avoided mindless scrolling, completed their tasks, and stayed aware of both their own and their peers’ usage, making social media use more intentional and focused.

