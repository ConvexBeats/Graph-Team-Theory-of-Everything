Rory Beattie
12:33:53
Hey, sorry, I'm a bit late.
Simon Hulbert
12:33:54
That's all right. It's all right. I double booked you, so I'll let you off. But yeah, so I was just explaining. So we've got meetings with Artificial later this week to try and tee up a plan.
Convex - Orega - 6B (Boardroom)
12:33:56
Okay.
Rory Beattie
12:33:58
Mm-hmm.
Simon Hulbert
12:34:09
Um, so that they're keen to adopt Party as part of their integrations, and we just want to make sure that, um, we've got… we know…
Rory Beattie
12:34:17
Yep.
Simon Hulbert
12:34:23
What is going to be available when, what we do in the meantime, and then make sure that, um, delivery of the…
the refactoring, effectively, is aligned with the September 1st.
um, deadline, I guess. So it's… what I wanted to do was get us together first to… to get all our ducks in a row and understand what's… what's happening, and then we can talk to them about it and make sure they're… they're happy.
and.
Convex - Orega - 6B (Boardroom)
12:34:54
And so, you know, Simon, artificial, the project manager guys that I work with are pushed back on the meeting tomorrow saying, oh, can we just send them a list of the questions? Can we?
Simon Hulbert
12:34:57
Yep.
Convex - Orega - 6B (Boardroom)
12:35:09
Can we send them the credentials for party? Can we send them all that documentation? And then they'll have a meeting at some point next week. I'm kind of… I've ignored it. We've got Sam, who is on… who's the FDA for integrations, who has accepted the call.
Simon Hulbert
12:35:14
Yeah.
Right.
Convex - Orega - 6B (Boardroom)
12:35:26
The other guys, I don't care too much about not being there, because I do want to just get some dates in diaries, and get some firm activities and tasks of what each party needs to do, really. Excuse the pun, but each.
Simon Hulbert
12:35:40
Here.
Convex - Orega - 6B (Boardroom)
12:35:42
Um, and then, and get that plumbed in, and then just start working towards some timelines. Um…
Simon Hulbert
12:35:43
Okay.
Convex - Orega - 6B (Boardroom)
12:35:49
And yes, if there are other timelines I need to be aware of, then if there's other things afoot for party, then that would be good to know.
Rory Beattie
12:36:00
Yep.
Cool. Um…
Simon Hulbert
12:36:03
So, go on. All right.
Rory Beattie
12:36:05
Oh, yeah. So, I just want to give Alex, because we didn't actually speak about this yesterday, just the context of why I'm on this call, Alex. Um, I'm sure you know it, but, uh, just to be totally clear about it. So, um…
Yeah, I think yesterday there was a little bit of a, um…
A little bit of a flag whiffed in the air as to a little bit of a.
we need some help just to coordinate this sort of thing, not from yourself, Alex, but from internally on the team, uh, technically. Um, and so that's… that's kind of why I'm coming here, Simon. So it's good to meet you, first of all. I don't think we've actually crossed paths yet, so it's… it's good to see you, albeit virtually at this stage. So…
Simon Hulbert
12:36:40
Thank you.
Convex - Orega - 6B (Boardroom)
12:36:42
Yeah.
Rory Beattie
12:36:45
I think one of the things on Alex and Mai, and Sergiu, and Joe, and the rest of the gang on the team, is just to get an idea of exactly what it is that you're asking for there, right?
So, um, I don't know how much of this has been made kind of clear to you about what's on the graph for the party team's backlog currently, and how that all sits together. Um, obviously a vast chunk of that, uh, backlog that they have, and Alex, step in here at any moment if I speak out of turn, or incorrectly, or whatever else.
But the vast majority of the backlog is focusing on this MDM solution, which is to provide the capability for you guys to access parties for high volume. So that's a big chunk of the roadmap. Now, in order to get to that point.
Convex - Orega - 6B (Boardroom)
12:37:25
Okay.
Rory Beattie
12:37:32
and this is the bit I'm not sure whether it's clear or not, but I'm really keen to make it clear across the entire board about what we're trying to do here, right? So, in order to get to that stage, we have an existing party service, which provides.
Convex - Orega - 6B (Boardroom)
12:37:37
Thank you.
Rory Beattie
12:37:46
parties solely to the in-risk application, and the way that it does that is, um, a pretty horrific integration in terms of something for repeatability, i.e. it's heavily embedded within the in-risk service, so as you can probably imagine.
pulling that out, um, is a bit of work, and the other option would be to heavily embed it within every other service, and then rip it all out later, which, quite frankly, just doesn't feel like a… an option going forward, right? So what the… the whole purpose of that MDM piece that they're trying to do.
is to try and make this a service in and of its own right that can be used by many people, right? So to do that, they're effectively rebuilding the entire backend of the application and the endpoints that deliver that party information outbound.
So it's a bit of an undertaking. So that's what we're trying to coordinate at the minute to try and work out what those steps are, what we already have, what we're missing, what's going to be ready and when, and then coordinate with people such as yourself, Simon, to say, okay, what do you guys need and at what stage, right? Because I assume at some point you're going to need.
At the very least, kind of a skeleton of what those endpoints schema look like so you know what fields you're getting and does that fit everything that you need? Is this something that you can play around with? Okay. When will that land? And and everything along that path. Right? It's not a service that we have stood up right now.
where you can just go, oh, okay, can we have access to a, you know, a trial version of it? We'll go and have a play around, and then, happy days, we'll just turn it on at the end of it, right? It's very much a, we're building the hull of the ship as we're trying to sail it out the harbor sort of job, right? So, there's a bit of coordination effort that we need to do between all of us, not just yourselves.
Convex - Orega - 6B (Boardroom)
12:39:23
Okay.
Rory Beattie
12:39:29
Obviously, the party guys and the guys over in the in-risk world, there's a few other stakeholders that are involved as well that we're trying to make it as small a footprint as possible. But because we're effectively tearing out the entire innards, that footprint is impacting a number of applications that we're having to just.
Convex - Orega - 6B (Boardroom)
12:39:29
Yeah.
Yes.
But…
And we've got a session with the InRisk team on integration with the Artificial Lab platform and InRisk. We've got that in about half one today to sort of like talk to them about activities required and things.
Rory Beattie
12:39:46
coordinate between.
Convex - Orega - 6B (Boardroom)
12:40:01
Does that impact them on the same scale as what it's impacting you?
Simon Hulbert
12:40:05
No. Well…
Rory Beattie
12:40:07
So, my understanding is, and Simon, you can probably correct me on this if I speak out of turn, but my understanding is that that's a small footprint, because you just need some information from InRisk. They're set up, they have endpoints that you probably can use straight away, so that's slightly different.
Simon Hulbert
12:40:10
Yep.
Convex - Orega - 6B (Boardroom)
12:40:15
Okay.
Okay.
Simon Hulbert
12:40:20
Yes.
Rory Beattie
12:40:22
This is… this is kind of a, like, uh… the dependency chain is pretty much undrawable, because it's such a spaghetti junction, but basically, because inRisk is interwoven with the party application, we need to decouple those two things. So it's kind of a separate piece of work, but it's…
Convex - Orega - 6B (Boardroom)
12:40:34
Okay.
Rory Beattie
12:40:38
It's related, because you guys are trying to use the new version of the Party app, right?
Simon Hulbert
12:40:43
Hello?
Convex - Orega - 6B (Boardroom)
12:40:43
육.
Rory Beattie
12:40:56
I mean, we…
Well, we are, and we kind of always have been aiming for that 1st of September date, right? I guess the little flag that was waved in the air just yesterday was, uh, hang on a sec, we might not be ready for that, right? And so our job is to go in and see if it's not going to be ready, what isn't going to be ready, right? And I think that's the…
Convex - Orega - 6B (Boardroom)
12:41:03
그러니까.
Rory Beattie
12:41:19
the bit, Alex, that we're just… we need to dig into, technically, to find out what it is that we have and don't have, and see what level to which we should be panicking, and trying to come up with a new plan, and a new strategy, and telling you guys about it, right? So, as far as I'm concerned.
our aim is still probably to try and hit that September 1st date. Obviously, Simon Hulbert, I did ask you yesterday whether that date was movable, and that's because.
You know, I just want to know all the tools I've got in my toolkit. But our intention is to try and absolutely hit that date, but we'll be able to tell you pretty soon whether that's reasonable or not, I would imagine, Alex, right?
Simon Hulbert
12:41:48
Yes.
I… I…
Rory Beattie
12:42:02
Yeah.
Simon Hulbert
12:42:02
And so, I got a YAML file, an instant face definition file from…
I think it was Joe sent it on, or…
Alex Sillars
12:42:14
Yep.
Rory Beattie
12:42:15
Probably would have been, yeah.
Simon Hulbert
12:42:15
So is that… because I sent that on to Artificial, and they said, oh, this looks good. Can we have a play with it? And I'm just thinking, do we have any… where are we at the moment in the process, the development process? Have we got stuff in…
Um, dev integration is… is in… where… where do we sit? What's the…
Alex Sillars
12:42:38
Yeah, I can answer that, if that's okay, Rory. So, currently in our dev environment, you are right, and soon to be integration, we have the DynamoDB set up with all of the party attributes that are part of that.
Rory Beattie
12:42:43
Yeah, I'd like to.
Alex Sillars
12:42:54
spec that was sent over to Artificial Value, Simon, so…
Simon Hulbert
12:42:58
Brilliant.
Alex Sillars
12:42:59
One of the things we're struggling with is the Boomi, um…
access for third parties, so I think Boomi is going to act as the interface between artificial and party. So, that's the bit that we haven't built out, but the database, the structure is there, so that if they were to call that API.
Simon Hulbert
12:43:09
Right.
Yes.
Alex Sillars
12:43:20
we would have fake data, admittedly, but data available for them to receive back. But there's still that work on security authorisations for them to be able to get a.
client ID and security token, um, back so that they can call it.
Simon Hulbert
12:43:34
Okay.
So we've got Srini, who's the Boomi Integration Architect, on the project.
So, I guess, who could I get him to speak to on the party side? Is it Joe?
Alex Sillars
12:43:48
Strive, yeah.
Simon Hulbert
12:43:51
So, right, okay, because he's done this before lots of times with lots of different providers, so it shouldn't… and all he's doing is effectively a pass-through, so he'll, um…
He'll get a link between Boomi and Party, and then he'll hand out the credentials to the, um…
the guys in artificial, and they'll be able to do that. So… so I guess that is a key thing that we need to…
Get stood up as soon as possible.
Simon, so we can talk to Srini about that, just to confirm that. So that will at least give them something they can ping to say, well, here's a look up of the parties. What's missing at the moment from the.
Alex Sillars
12:44:35
Okay.
Simon Hulbert
12:44:36
The work is it the curation or is it what's what's the.
Alex Sillars
12:44:37
Yeah, so currently missing. So we have a bare bones curation part of it that needs to be extended with all the features for curation because they are deeply coupled. We need to migrate existing data from Neo4j ParseDB into the DynamoDB.
Um, and crucially, we need to think about how we then emit events through to the DU sanctions, so that they're encompassing all the data that we currently do. So, um, we know if we're not going to store some submission IDs in Party, we still need to.
Send that as part of our event. So where do we get that from? So that's the core of the work that's still being built out. So it's not necessarily the front facing part of your world, artificial.
Simon Hulbert
12:45:11
Thank you.
Right, okay.
Alex Sillars
12:45:24
For that, we're in a good place, but all of those things would need to be delivered in order for us to say we can go live with the full solution, yeah.
Simon Hulbert
12:45:33
Right, okay. And we've got a meeting tomorrow about how we do sanctions.
Alex Sillars
12:45:38
Uh, yep, and that's… that's the new, new world, I want to hasten to add, Simon. Our… what we're delivering for this is…
Simon Hulbert
12:45:39
In the new world.
Sure.
Alex Sillars
12:45:47
Everything that's going to come out of Party after we go live on the 1st of September is exactly what it comes out today. We're going to do that in a different way. We're going to obtain that information through a different source. It might not be Party, it might be joined up in Risk and Party data, but it will still hit sanctions, DU, in exactly the same way.
Simon Hulbert
12:45:57
Right.
Good.
Yeah, okay.
Brilliant. Okay, so I've got some ideas on how that might work, but…
Alex Sillars
12:46:09
Awesome.
Simon Hulbert
12:46:12
But yeah.
Um, I think the only possible impact that…
Well, so what we've got to realize is that InRisk isn't going to be the only person or only application interested in this stuff.
now. So we have to make sure that if we flag it as something that Artificial is interested in, then we have an easy way of routing that to Artificial. So that's my key thing. So if, for example, sanctions gets.
A party that we're onboarding gets sanction checked, and there's a failure or a pass.
We need to just…
make sure we can update that in Artificial straight away, um, in an easy way. So… so yeah, um, there's a bit of work, but I think, from my perspective, if we've got data in the DynamoDB.
And.
file some test data or something that we can start to use, then I think we get them set up so they can access that.
Um, via Boomi.
And once… once that's done, they can go away and play for a while, and we just then, on our side, we track the deliverables to understand the second bit of the integration, which is, if a party changes.
We need to update.
artificial with that data.
Alex Sillars
12:47:48
Yeah, and again, that for us, we're not expected to do any work. We're going to omit events. We're going to tell you the party role is a cover holder, and we're going to expect you to update the data. I can hear Joe screaming in the back of my head just saying that.
Simon Hulbert
12:47:55
Jeg.
Her.
Alex Sillars
12:48:04
But yeah.
API spec that we provided is subject to change until we go live, which sounds like an obvious thing, but important to say out loud. It won't change massively, but there may be some.
some additions or deletions from it, but yeah, I think for testing purposes, suitable.
Simon Hulbert
12:48:21
Her.
Så så kan.
I så fald ikke som The Solution Side of Things.
We Figaro. How.
Notification, So.
Just native get a bit more information.
So… but we just need to track the… the delivery of the rest of it, and… and just keep this conversation going to make sure we're…
I'm sure Artificial will come back with some questions or things that they don't understand, but they…
Yes.
Convex - Orega - 6B (Boardroom)
12:49:42
a party bespoke.
integration channel with just the FDEs that we were going to work with.
Simon Hulbert
12:49:51
I don't know.
Convex - Orega - 6B (Boardroom)
12:49:52
Or do we not need a channel?
Simon Hulbert
12:49:53
Uh, I think we set up some…
Convex - Orega - 6B (Boardroom)
12:49:54
but.
Simon Hulbert
12:49:56
Well, if it's… I mean, this is slightly sporadic, so say if you don't want to do this, but if we've got…
Um, so we get, we'll get Srini to, um…
to… to set up the Boomi integration. He works on that, so that at least we get through to, um…
the party from artificial, and that's the first goal. Once that happens, we'll get the techies talking.
Convex - Orega - 6B (Boardroom)
12:50:20
Yep.
Simon Hulbert
12:50:24
Um, and I…
I can set up meetings, or I can get them to set up meetings to… to discuss questions, or problems, or those types of things.
Once they've had a chance to explore. So, so, um.
That's… yeah, just getting them together in a meeting room or something like that might be useful. We could get a virtual meeting room.
Convex - Orega - 6B (Boardroom)
12:50:49
That's… that's… that's… that's… that's fine. Um, I'm just conscious who's gonna track…
Simon Hulbert
12:50:49
Maybe.
Yes.
Convex - Orega - 6B (Boardroom)
12:50:56
Actions and bits and bobs and things like that, um, and…
Um, do we want a shared channel, a Slack channel, artificial or on Slack? So, do we want a channel to have the conversations, or any offline chat?
Simon Hulbert
12:51:10
Yeah.
Convex - Orega - 6B (Boardroom)
12:51:13
out of meetings. How's best to do that again? I'm conscious. It's not. It's another channel. I'm I'm keen to throw all of these integrations into one channel. To be honest, let me just put a rule saying, say, which which application we're talking about.
So you put that at the first word of the.
Of this, of the chat, of this, of the chat or the message.
I don't know. I'm open to it. I'm open to… I don't want to flood people's…
Simon Hulbert
12:51:38
Let's…
Convex - Orega - 6B (Boardroom)
12:51:44
Slack channels, either.
Simon Hulbert
12:51:47
Let's give it a go. Let's try that. And if it's… if lots of people are working on that, then great.
Convex - Orega - 6B (Boardroom)
12:51:53
Yeah, please.
Simon Hulbert
12:51:54
Uh, and… and see, yeah. Because I think, um…
Convex - Orega - 6B (Boardroom)
12:51:56
Bye.
Simon Hulbert
12:51:58
I think it's good that… I mean, Srini's gonna be involved in lots of this anyway, so he's gonna be a central point, so I think we need to get him involved, um…
Convex - Orega - 6B (Boardroom)
12:52:02
Yeah.
Simon Hulbert
12:52:10
As part of the team as well, and then we can.
Convex - Orega - 6B (Boardroom)
12:52:11
Yep.
Knock on the stand ups.
Simon Hulbert
12:52:14
Yes, I think so.
Convex - Orega - 6B (Boardroom)
12:52:15
Yeah, okay.
Okay.
Simon Hulbert
12:52:17
And then… right, okay, so we've got that side of things sorted, and then do we need a regular catch-up to… or how do you want to…
Talk to us about how things are going.
Roaring.
Rory Beattie
12:52:30
Yeah, I think we probably should have a regular catch up, right, Alex? I mean, maybe every couple of weeks for now, and we can catch up on the channel between, but obviously close to the time we might make it a bit more frequent.
Alex Sillars
12:52:32
Taroga.
Convex - Orega - 6B (Boardroom)
12:52:46
Okay, to put fortnightly catch-ups in there. Do you want artificial at those catch-ups as well? We get the relevant artificial people in there, or you want to keep it this side of the fence?
Rory Beattie
12:52:46
And…
I'm gonna…
Good question.
Convex - Orega - 6B (Boardroom)
12:52:58
Okay.
Rory Beattie
12:52:59
What do you reckon, Alex? I'm tempted to say include them, but…
Simon Hulbert
12:53:05
Well, so here's a question. Um, is it technical, or is it planning? Is it something that you just want to share the progress and… and make that… get the planning people in…
involved from the artificial side? Is that what we want to do?
Rory Beattie
12:53:21
The reason why I said include artificially is because I don't want the guys to have to explain it twice.
That's… that's where my head was at, right? Um…
So the answer in my mind is that it could be both.
Um, we could, you know, speak technically and say when that technical aspect of it is arriving. Um…
Simon Hulbert
12:53:41
Okay.
Rory Beattie
12:53:42
But, Alex, I'm very keen. I've been here 2 seconds, so I don't wanna, you know, come and…
you know, interrupt your kind of plans that you've had in place so far. So what are your thoughts?
Alex Sillars
12:53:57
I would get artificially involved. I think all of us are semi-planning technical, so kind of makes sense that we can talk about those levels with them.
Convex - Orega - 6B (Boardroom)
12:54:01
Okay.
Rory Beattie
12:54:03
Hmm.
Convex - Orega - 6B (Boardroom)
12:54:05
Okay.
Rory Beattie
12:54:06
Cool, okay.
Do you as well, Simon, on a — sorry, Simon Holbert. This gets confusing pretty quick for you guys, I imagine, right? Do you have a diagram that I can point at that gives the overall idea of what it is that high volume are trying to do?
Simon Hulbert
12:54:15
Yeah, I love that.
Rory Beattie
12:54:26
And where, I guess, the touch point with partying. That'd be really useful for me, just to know what I need to make sure lands and when. Also, it's just the other side of that same handshake.
Simon Hulbert
12:54:31
Yeah, sure.
Yeah, sure. I can… so…
Rory Beattie
12:54:37
I'm sure you do. You know I'm going to have.
Simon Hulbert
12:54:40
So I've got the big picture, and then I've got the sequence diagrams where you see the individual things we're going to ask for from…
And.
party as well, so I can… let me send on the top… top level one first, and then I can go into a bit more detail if that works.
Rory Beattie
12:54:57
Yeah, no, that sounds cool. If I'm feeling brave, I'll dig into a bit more.
Convex - Orega - 6B (Boardroom)
12:54:58
Thanks.
Simon Hulbert
12:55:00
Stick.
All right. Well, yeah, feel free to shout on Monday, Wednesday, and Friday. I'm always around, so yeah, happy to go through that.
Rory Beattie
12:55:01
Thank you.
Perfect.
Yeah, excellent. No, thank you. And then, uh… yeah, that'll just help us paint the picture of, as I said, what needs to land and when, which I think is just the little hurdle that we need to get over initially, right, Alex? Um…
But I don't want to raise too many alarm bells, because I don't feel that alarm, it's just because it has been raised within the team pretty recently, and I just want to make sure that.
we've got a big chunk of what needs to happen, right, already there. It's just that there's a lot of peripheral stuff, and I think that's where the little bit of, like.
Simon Hulbert
12:55:37
Oh.
Rory Beattie
12:55:42
it's not strictly the critical path that we're worried about, I don't think. I think it's the peripheral sort of integrations and the curation bits and whatever else that we need to have land at the same time, from our aspect, which is what we're actually a bit more worried about, right?
Simon Hulbert
12:55:46
And… and…
Alex Sillars
12:55:53
Yep.
Simon Hulbert
12:55:57
And you talked about in-risk integration.
I'm presuming that goes first, or has to go first?
Rory Beattie
12:56:06
Correct, yeah, yeah. So, uh, our understanding as it lands at the minute, which I think is pretty firm now, Alex, right, is we know what we're doing within risk, right? So we're putting in an intermediary step, which means that we don't have to do as much within risk as we might otherwise have had to do. They are working on that right now.
Alex Sillars
12:56:12
Yep.
Simon Hulbert
12:56:12
Hmm.
Rory Beattie
12:56:25
Um, to have a look at that. Um, so that should be ready sooner rather than later, but again, we need to flesh out the MDM solution for them to be able to fully proceed, right? But we talked about it happening earlier than September 1st, because.
Simon Hulbert
12:56:25
Okay.
Hmm. Okay.
Rory Beattie
12:56:41
thing we don't want to do is just switch everybody on all at once and run the potential for it going wrong, right? We don't want to go from one user to a million users, all using a different version of the same thing and not be able to not have a rollback strategy, right?
So, our intention is to move those at least a couple of weeks, I think, ahead of when we think we'll be able to land with you guys. Obviously, that date's just to be confirmed.
Um, but yes, earlier, TLDR, earlier are in risk than for you.
Simon Hulbert
12:57:11
Ma'am.
Right, and, and…
So, we're tracking their, kind of, dependency as part of this. So, it's the party stuff, but then we want to get in-risk over first.
And they'll deliver, and then once they've delivered, we will roll out shortly after that.
Rory Beattie
12:57:31
Yeah, and it's, uh, we don't want to… we unfortunately have to. Yeah, yeah. So it's, uh, yeah, because we can't run two solutions at once.
Simon Hulbert
12:57:34
We've got to. Yeah.
Rory Beattie
12:57:42
it's just not feasible. We have to turn on the new solution, and that means eRisk have to be using the new solution, and because we only want you to use the new solution, that's why you guys come second, right? A close second, but second, nonetheless.
Alex Sillars
12:57:44
Thank you.
Simon Hulbert
12:57:48
Yeah, yeah.
Yeah. Okay.
Rory Beattie
12:57:55
Yeah. Okay.
Convex - Orega - 6B (Boardroom)
12:57:55
Okay.
Rory Beattie
12:57:57
Um…
So hopefully that's cleared up a little bit as well, like some of the things that these guys are up against just to, uh, because I think it's important just to know exactly what's going on on both sides of the world, right? I'm sure you guys have got things that you've not shared also that are, you know, like pretty pressing that are not directly affecting party, but you're a little bit worried about.
Simon Hulbert
12:58:09
Yep.
Yeah.
Rory Beattie
12:58:15
So I think just always being open and honest and whatever else about things this early on, I think is can only be a good thing.
Simon Hulbert
12:58:16
Yeah.
And I guess just to be upfront and honest, as far as the artificial side of things goes.
They measure their development or configuration in their world in minutes and hours, or hours and days, rather than… and they're like, well.
Convex - Orega - 6B (Boardroom)
12:58:40
Yeah, thank you.
Simon Hulbert
12:58:43
If everything's lined up perfectly, then it could take us…
Minutes to do this, get this system working, which is…
Rory Beattie
12:58:50
I'm sure that happens all the time, right?
Simon Hulbert
12:58:52
Yeah, yeah, exactly. So, so if we can just get it all perfect before they start, then… and that's, that's what they're doing at the moment. They've spent a lot of time thinking.
Rory Beattie
12:58:57
Yeah. Okay.
Simon Hulbert
12:59:03
And now they want to start and they're like, well.
Give grant us access to all of your things, and as they will be perfect, we will.
Um, so there's quite a condensed…
Rory Beattie
12:59:13
If they can tell us what their perfect 3-pin socket looks like, we can almost certainly build the equivalent 3-pin plug to go in it, right? But, uh…
Simon Hulbert
12:59:18
Yeah.
Yeah, so it is condensed timelines on their side of things, so they haven't got long.
Convex - Orega - 6B (Boardroom)
12:59:23
So…
Rory Beattie
12:59:23
Yep.
Yep.
Convex - Orega - 6B (Boardroom)
12:59:29
Bye.
Simon Hulbert
12:59:30
And their expectations are high, so they're pushing to get integration with a…
APIs as soon as possible, and that's kind of…
And it sounds like we can meet that initial thing, provided we get Boomi lined up, so they can set up the connectivity. Um, it's just going to be the… with the caveat that these things may change slightly.
um, through the development life cycle. And the… the bit where the feedback loop goes back round to artificial from party, when the party has changed.
That's gonna be a slightly later delivery, and that's…
At least we've given them something to work with in the meantime. I guess that's the key thing. Okay, brilliant.
Alex Sillars
13:00:15
Yep.
Simon Hulbert
13:00:19
I hope that's helped everybody. We know, well, it certainly helped me get visibility, but, um…
But if you can put those sessions in, Simon, or get Luca to, then we'll catch up every…
Convex - Orega - 6B (Boardroom)
13:00:33
Yeah.
Every fortnight.
Simon Hulbert
13:00:35
Every. Yeah. Okay, cool.
Convex - Orega - 6B (Boardroom)
13:00:36
Yeah, we'll put those in. And that's just Alex and yourself, Rory, yeah? Is there anybody else that would need to be in those? Does Srini need to be in them?
Rory Beattie
13:00:43
But…
Simon Hulbert
13:00:46
I… I…
Rory Beattie
13:00:47
Uh, it might be worthwhile, yeah. And then, if there's anyone else, we can throw them on Alex, right? Whether we need Joe or Sergio later down the line, we'll, uh, we'll cater for that.
Convex - Orega - 6B (Boardroom)
13:00:47
or…
Simon Hulbert
13:00:49
Yeah, yeah.
Convex - Orega - 6B (Boardroom)
13:00:50
Yeah, okay.
Alex Sillars
13:00:53
Yep. Yep.
Convex - Orega - 6B (Boardroom)
13:00:53
Yeah, sure.
Simon Hulbert
13:00:58
Good stuff.
Convex - Orega - 6B (Boardroom)
13:00:58
Okay, no worries. Excellent. Thank you very much.
Simon Hulbert
13:00:59
Right.
Alec's going to have a quick run round. Get out of the chair.
Rory Beattie
13:01:02
Thank you, guys.
Alex Sillars
13:01:03
Yeah, thanks, mate. Stretch that back.
Cheers, guys!

