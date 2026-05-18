Joe Worsfold
11:06:43
So we wait for anyone else.
Rory Beattie
11:06:46
Yeah, I think we're… I think we're probably there. Sorry, I was a bit late, guys. I'm also having horrendous internet problems today, so if I cut out randomly, I apologize in advance.
Um…
So, I just wanted to try and use this time just to pick up the reins a little bit on where we were. Um, I think I'm most specifically concerned about the in-risk bit just for this particular session, um, and how that works. Obviously, um, Joe.
It's super useful to… for you to be here in terms of the interface points between those. But I'm… I'm super interested in finding out exactly what, um, John, Chris, you guys have together currently in terms of what you think needs to be done.
Um, and when that might land, and yeah, I was about to say, John, I know you've been doing a bit of work on this, right? Um…
John Trahearn
11:07:42
Yeah, so, I mean, hot off the press, basically, for this call, it's one of those things that was on the to do list, and Lloyd's two was sort of in the way as well. But basically, I've condensed kind of what we spoke about in the previous call into like a high level epic, so party MDM integration.
And then we've got 4 stories that sit below it, which we need to then obviously take through a high level, low level type thing.
Rory Beattie
11:08:04
Yep.
Yep.
John Trahearn
11:08:11
Um, so, uh, story number one is what we noted is sort of a bit of technical debt really, but we need to remove the manual client creation on new requirement. So currently that's still in play. And if that's still in play, we sort of.
slightly outside the bounds of kind of our agreed starting position. So this story is something we could take straight away into a sprint and integrate with the widget.
to manually create things within the widget itself, not falling back to the in-risk pages. So that's that one.
Rory Beattie
11:08:48
And, and that, that absolutely exists right now in the widget, right? We can utilize that right at this exact second, it's just that we're not utilizing it because there's a little bit of tidy up that we need to do, which is basically the scope of this ticket, yeah.
John Trahearn
11:08:58
That's my belief. Yeah, that's my belief. We use that in other areas. So unless someone calls out that it's not going to work, I don't see why not.
Rory Beattie
11:09:01
Okay.
Yeah. I… yeah, that makes sense. Cool.
Kris Mokrzycki
11:09:15
And John, have you checked, is it exactly the same widget? Like, I know, you know, in case it's a different flavor of the widget, because I'm not that familiar with widgets on… of parties that we use.
John Trahearn
11:09:27
I've not dug deeply into it, but, um…
Kris Mokrzycki
11:09:29
Yeah.
John Trahearn
11:09:32
Yeah, um…
Yeah, without sort of digging slightly deeper, don't know, but I believe it is.
Rory Beattie
11:09:39
Mm-hmm.
Joe Worsfold
11:09:39
Yeah, should we… if we're gonna bring it to high level on Thursday, we just invite Billy, who's the, sort of, local expert on the existing party search widget, and then we can just validate fine fairly quickly, I imagine.
John Trahearn
11:09:39
I think there's anyone contextual.
Okay, yeah, perfect.
Yeah, so if I bring this to high level and Billy's on the call, uh, that would be ideal, actually.
Rory Beattie
11:09:58
Cool.
John Trahearn
11:09:58
Because we'd like to get this probably going.
Kris Mokrzycki
11:09:59
And…
John Trahearn
11:10:01
probably into next week. We've got high level Thursday and low level Tuesday following, so it could be started the next sprint.
Rory Beattie
11:10:08
Yep.
All right, cool. So that's just a little assumption that we've made, but we need a decision on it just to make sure we're on the right lines, but it sounds like we are.
John Trahearn
11:10:10
And…
Yep.
Yep.
Rory Beattie
11:10:18
Excellent.
John Trahearn
11:10:19
So then, uh, there's 3 stories that I think cover the work. I don't think the work's, like, particularly vast, to be honest.
Rory Beattie
11:10:26
Not for Phase 1, right, for you guys?
John Trahearn
11:10:26
And…
Not for phase one. So ultimately, there's updating the data model to support storing party ID and version IDs on our client or on our existing client and broker table. So the idea of this is.
We want to stay backwards compatible, we want to change the model as little as possible, but support these new IDs coming in. So the agreement was keep in risk as is, we'll still store the snapshots and all the data associated with them, but we'll store the IDs.
related to them when we integrate with the new MDM solution, right? So this covers that off in terms of the data model on the database, which is a straightforward change, and kind of any DTOs and stuff within our ecosystem that need to be updated.
Rory Beattie
11:11:04
Yep.
John Trahearn
11:11:16
Um, and also…
Rory Beattie
11:11:16
Gotcha. Does this not cover off? So let me think about how to phrase this.
this is purely for those new pieces of information that we know are going to come through within the new version of MDM, right? It doesn't cover off the fact that we will still need to extract the same fields that we do today from whatever the new feed is, right? It doesn't cover that.
John Trahearn
11:11:30
Yeah.
Correct.
Rory Beattie
11:11:42
Excellent. All right, cool. Yeah, yeah, just so.
John Trahearn
11:11:43
No, no, so this is just purely putting us in a position where we can store the additional things as well as what we currently store.
Rory Beattie
11:11:50
Yeah, okay.
John Trahearn
11:11:51
And then basically there's two stories then, one for clients and one for brokers, which I thought made sense. It might be that it boils down to it being quite straightforward to do it as one, but.
Rory Beattie
11:11:59
Yep.
John Trahearn
11:12:04
Um, I mean, these are fairly AI-generated at the moment, but, um, so this is ultimately the integration with the new widget, um, so feature-flagged.
Um, in terms of when the feature flag's on, we'll display the new MDM widget. When the feature flag's off, we'll just, you know, we'll launch the widget as it stands now.
But ultimately, the crux of this is, when it's on and the new widget's there, we'll do everything that we do with the current widget, but we'll also pull back those new IDs and persist them, so that they then flow through the system.
Rory Beattie
11:12:36
Thanks. Yeah.
John Trahearn
11:12:36
And likewise for brokers.
Rory Beattie
11:12:38
Yep.
John Trahearn
11:12:39
So, yeah, so…
We've got the epic, we've got four stories, which I think covers all of the work.
And yeah, we're at a position now where we can start pushing these through refinement sessions.
Rory Beattie
11:12:53
So, the as-is state…
Joe, for the widgets, it sounds like we have multiple widgets that basically do the same thing, but just with a different slant of party, right? So we've got client, we've got broker.
I'll come to feature tagging in just a second, but we have different widgets, right? In the new MDM world, when we get there.
Will that be one widget, or will that still continue to be multiple widgets?
Joe Worsfold
11:13:25
So I think it's basically the same as the existing one, but.
Whilst it is multiple widgets, it's more like multiple components that wrap.
uh, slightly different calls to the… to the search, right? So in the new world, it's OpenSearch just wrapping different calls to either, like, search party with broker roles, or search party…
as insured or reinsured. So that… it's pretty similar, it would just be use different components for the different…
searches.
John Trahearn
11:13:52
You make a good point. Actually, we might want a third integration story for party tagging.
Joe Worsfold
11:14:09
Gotcha. Okay. Yeah.
Do we need. Yeah, we'll need some stuff on our side as well. Like the model supports it, but there's no integration points yet for tagging via MDM.
John Trahearn
11:14:33
Yep.
Joe Worsfold
11:14:37
Yeah, exactly.
Yeah, and I guess we haven't talked about it yet.
Yeah, because it's… it will work fine as is, so probably out of scope, but uh, when we want to turn off the old party, I've accounted for tagging, and I've accounted for, like, session tracking, I just haven't accounted for feature.
Feature tagging, not party tagging.
Rory Beattie
11:14:56
Hmm.
Joe Worsfold
11:14:57
um…
But it's fine, potentially we can leave it out of scope, but we'll need to deal with it at some point, so I can delete old party.
Rory Beattie
11:15:08
Um…
Sorry, I'm gonna ask a stupid question now, then.
What's the difference between party tagging and feature tagging?
Joe Worsfold
11:15:16
Like you can use, uh.
Tag a party is like an obligor or something right on.
In InRisk, but you can also then do additional feature tagging to a submission, which is sort of irrelevant of party, it's other…
other features. It was just developed by Party because it followed a similar sort of UX journey, but actually it's nothing to do with parties.
But it's stored in the old RT, yeah, tables, so.
Kris Mokrzycki
11:15:39
Yeah, that's what I'm excited. Yeah.
The feature tagging is totally different to the rest, right? Because I don't think it has to do anything with the party. It was just conveniently provided by the party team.
Joe Worsfold
11:15:51
Yeah, exactly. It's an unfortunate sort of, obviously we're trying to domainify this better, but yeah, at the moment, party absorbs. I mean, there's nothing to do with parties to just enable some.
similar UX in the… on the inner-ish side. Like I said, though, like, we won't decommission old parties straight away, but it's on my list of, like.
That has to be resolved. Just as similarly, we have to resolve some sanctions based stuff as well, which I've been talking to the Boomi guys about.
Because those sanctions check parties and submissions is like a together, which means.
Rory Beattie
11:16:20
Yeah.
Joe Worsfold
11:16:26
That was part of why.
Marty has so much information about.
in-risk stuff, but it goes back to the problem of, like, who's responsible for the merging of a submission and a party to send it to get sanctions checked, but…
Probably.
It's actually the one we have to sort out, but the feature tag you might have to come back to.
Kris Mokrzycki
11:16:47
But didn't we say last time that actually that is out of scope for now, right?
the feature target.
Joe Worsfold
11:16:52
That's it.
Yeah, I think we said feature setting would be.
Kris Mokrzycki
11:16:55
Yeah. Okay.
Joe Worsfold
11:16:56
Yeah, it's just a…
Rory Beattie
11:16:56
Okay, so I think I said feature tagging, right? And maybe I meant party tagging.
Suzanna Whitefield
11:16:57
Yeah, making any changes to it.
Joe Worsfold
11:17:03
Yeah, but it's… I think the ticket's right to just integrate party tagging is… is in MDM.
Rory Beattie
11:17:08
Yes.
Fine, okay.
So, feature tagging, it's just, um…
I'm just gonna repeat it so I'm explicit here. Feature tagging exists purely in the in-risk world. This is all of that, um, extra database tables that, Chris, you put together a year or two back, right?
Kris Mokrzycki
11:17:30
Yeah, but I wouldn't call it purely in InRisk. Maybe InRisk is the only one using this, however, it is predominantly driven by the…
I don't want to call it party team, but, you know, the party infrastructure, there is a database somewhere, there is some service behind the scene, but yes, you're right, it's probably we are the only one using that.
Rory Beattie
11:17:45
Yeah.
What I thought.
Yeah, but it still uses… it still uses a widget to pull some of those things, right?
Kris Mokrzycki
11:17:54
the type assist.
Correct, yes. And the service is behind the scene.
Rory Beattie
11:18:01
So…
Joe Worsfold
11:18:02
Yeah, but yeah.
Rory Beattie
11:18:03
How do you keep that out of scope for phase one then, Joe, from your side?
Joe Worsfold
11:18:07
Well, I can't decommission old party until we get rid of that, so we'll still end up having to have.
the table, the API and the old widget for enabling feature tagging until we get rid of it.
Rory Beattie
11:18:22
I see. So…
Suzanna Whitefield
11:18:23
What? And what does it? And what does it mean? If.
Worst case scenario, there is pushback for finding a new home for feature tagging.
How does that impact this project?
Joe Worsfold
11:18:36
I really, really need to decommission the old party sooner rather than later, because it's an absolute nightmare to maintain something that's on, like, Node 16, which has been out of support for quite a long time.
So, worst case scenario is, I'll build a really small microservice to do it, but I don't want to own it, because it has nothing to do with parties, and that's part of the…
John Trahearn
11:18:53
I was gonna say, so who owns it then? Because it doesn't make sense for you to rebuild it.
Suzanna Whitefield
11:18:55
RT.
Rory Beattie
11:18:57
Well, Patty owns it right now, but is that the right home, I guess, is Joe's question, because if he doesn't have any…
John Trahearn
11:18:58
Yeah, but it doesn't make sense.
It's not…
Yeah, yeah.
Kris Mokrzycki
11:19:03
So yeah, that's what I would comment also, because it feels like it should come into space of in risk, because we are the only people who consume and surface that.
John Trahearn
11:19:11
Pretty blind.
Kris Mokrzycki
11:19:15
The trouble with that is, was that it came under with some requirements at the time when it was the party who was configuring the questions and the answers and possible like a polling yearly.
mechanism that is controlled from outside. If those business requirements goes away, like, you know what, we do not actively maintain it, or.
it doesn't have to be dynamic, or whatever this is, right? We probably could accommodate, because it belongs solely to in-risk, and it makes no sense to be outside.
Rory Beattie
11:19:49
Yeah.
Kris Mokrzycki
11:19:49
Unless this has to be dynamic, as we said.
Joe Worsfold
11:19:50
Yeah, okay.
I think the way, the way I always saw it was that these features are features of a submission. They have nothing to do with party. Party tagging obviously does because it's tagging, it's like obligors or loss leader or whatever else. But yeah, the features were always submission specific. So I would obviously say it's in risk, but then.
Suzanna Whitefield
11:19:54
Uh, no.
Kris Mokrzycki
11:19:59
Yeah.
Rory Beattie
11:20:02
Yeah.
Joe Worsfold
11:20:09
That's…
one for, I guess, the business to decide, uh…
I definitely think Antoine should be… have it in mind in the future, but…
Rory Beattie
11:20:18
Yes.
Joe Worsfold
11:20:18
It's just an unfortunate thing. I think it was just the case of the reactive decision at the time was use the party UX because it's.
John Trahearn
11:20:25
Yeah. Okay.
Joe Worsfold
11:20:26
La relación de.
John Trahearn
11:20:29
I can't remember which came first, tagging or… feature tagging or party tagging, but it was just a, hey, we've got this system already, let's build on top, uh, this same pattern, because it'd be really quick.
Rory Beattie
11:20:30
Can I? No.
Joe Worsfold
11:20:36
Ya.
Rory Beattie
11:20:36
You're done.
Joe Worsfold
11:20:37
Exacto ya.
Rory Beattie
11:20:38
I think feature tagging came first, didn't it? But party tagging.
Suzanna Whitefield
11:20:42
Bye.
No, feature tagging has come out… came out of CDM, Rory, I think.
Rory Beattie
11:20:48
I don't think so. I think it was… Wasn't it Scott's and Sarah Faraway kind of…
Suzanna Whitefield
11:20:48
where we needed…
Rory Beattie
11:20:56
took Chris, you and I, to one side to implement feature tagging a few years ago.
Suzanna Whitefield
11:21:00
This is a Ringo special. Yeah, yeah.
Rory Beattie
11:21:02
Yeah, yeah, exactly, but I don't think it was CDM, was it? It was… it seemed to predate CDM.
Suzanna Whitefield
11:21:05
Yeah, it… it…
No, it's all part of the CDM world that we needed to capture additional attributes and things. Sorry.
Rory Beattie
11:21:15
Interesting. So, just to bring, um…
Just to, again, try and summarize, if I may, just so I'm totally clear on this.
Sorry, if we've gone around the houses a few times, but the party tagging bit…
Chris, question for you is, that's the party role in table that was set up with all of the multiple options that you can select, and that's attributes of a submission that a party can play a role in, is that correct?
Kris Mokrzycki
11:21:48
Yes, I believe so.
Rory Beattie
11:21:49
Yeah.
Cool. And then…
feature tagging.
Um, is a similar functionality, but nothing to do with an actual party, it just…
tag-specific information, using the same structure of database table within InRisk.
But they're separate database tables, but they follow the same pattern. Yeah, yeah.
Kris Mokrzycki
11:22:10
Pretty much, yeah.
And we use the way of, you know, presenting that already on the page, so you're right, we just reuse common components, but I believe those serve totally different purposes, yeah.
Here it is, I think. Who's showing that? I think that's the the example of it right?
Rory Beattie
11:22:27
Yeah.
Kris Mokrzycki
11:22:28
Yeah.
Yeah, feature tagging is, yeah, just a bunch of select attributes. Yeah.
John Trahearn
11:22:42
Is this a finite list of things, then, like the re like? This doesn't seem something that's particularly difficult to migrate away from this into.
Into pre-buying world, right?
Joe Worsfold
11:22:55
Eso es ahí.
Kris Mokrzycki
11:22:55
It doesn't.
It doesn't, but as I said, the requirement was, right, John, that we do not, so sorry, whoever drove that requirement said we don't want to bother in-risk for, let's say, a new option. This is why it's fully.
Suzanna Whitefield
11:23:13
Mmhm.
Kris Mokrzycki
11:23:14
Conflict driven.
John Trahearn
11:23:15
Oh, yeah, yeah, yeah.
Kris Mokrzycki
11:23:16
And then so, so if we can say that then essentially it's a static list that now settled.
it's straightforward, right? However, if it still requires a UI and management page, an easy access, an easy… you see what I mean? All those things, then it goes beyond, you know, InRisk itself.
John Trahearn
11:23:24
Yeah.
Because you could easily do this with classifications. Classifications have a hierarchy, but obviously that's then managed within in risk, but.
Kris Mokrzycki
11:23:37
Because it becomes…
Yes, you could.
Yeah, but how do you manage classification? At the moment, it's a nightmare. Plus, you can screw so many things.
John Trahearn
11:23:47
But…
It would be a manual… well, an in-risk change to add or remove, or change names, or whatever, but…
Kris Mokrzycki
11:23:53
Exactly.
Suzanna Whitefield
11:23:54
I think, so my remembrance of this was that ideally all of this information would have gone into INRIS, but the challenges of implementing it as separate, you both will understand this far better than me, but the challenge with implementing additional fields versus the time in which the CDM project wanted this available.
Meant that we leverage party.
John Trahearn
11:24:17
Yeah.
Suzanna Whitefield
11:24:18
Now, whether we can say now, Rory, rightly, two years on in the project.
are any additional attributes being added to this? Probably it's a no. Um, and can we use it as a static list? But I don't know. I wonder if we take this away and do a bit more investigation about where this could…
John Trahearn
11:24:34
Yeah.
Suzanna Whitefield
11:24:39
Live.
So then, Joe, also, then you have comfort about, um, the impact on the re-architecture, and then the timelines on decommissioning. Old party. Does that make sense?
John Trahearn
11:24:49
Yep.
Kris Mokrzycki
11:24:53
Yeah, it does.
Joe Worsfold
11:25:05
Sí.
De.
C.
O.
Well, basically, just a spike to…
To use Tracker 3 in Henrisk, I don't know what it's going to be like.
John Trahearn
11:25:31
I think, I think Chakra 3 and InRisk is slightly complicated, if I'm not mistaken.
Rory Beattie
11:25:36
Mm-hmm. That's what… we haven't touched it because of that reason, yeah, right now.
John Trahearn
11:25:40
Yeah, I…
Rory Beattie
11:25:42
I mean, from what I know, Chakra 2 in risk is also slightly complicated.
John Trahearn
11:25:46
Yeah, I was gonna say, maybe Bishak could too.
Rory Beattie
11:25:47
and…
Kris Mokrzycki
11:25:47
Okay.
Rory Beattie
11:25:48
I'm almost tempted.
and I don't know if this is the right answer or not, but I'm almost tempted to say build it in…
something a bit more raw than a design system, so we're not tied to it, and then changing it later. That seems like a horrible sentence to say, so please don't hold me to that.
John Trahearn
11:26:05
You go ahead.
Rory Beattie
11:26:08
But I don't know whether…
the… putting it in Chakra 2 actually gives us more work further down the line than if we just…
didn't, you know what I mean? Because at some point, we'll probably have to migrate to Chakra 3, and is it easier to migrate Chakra 2 components, or is it easier to migrate the bulk of the codebase, which isn't on Chakra at all?
Um, don't know the answer to that.
Kris Mokrzycki
11:26:30
But are you saying chakra, or are you talking about a design system?
Rory Beattie
11:26:34
It's… it's effectively… they're synonymous with each other, right? So…
Kris Mokrzycki
11:26:38
They are, but we don't use any design system on NRISC, right? Which means that it's not the same thing.
Rory Beattie
11:26:42
No, but you do use Chakra 2, right? You use some components of Chakra 2, they're just not design system components, which is, this whole thing's a nightmare, an absolute nightmare.
Kris Mokrzycki
11:26:50
Yeah.
Yeah, that's what I think. We shouldn't consider chakra two, three components. We should just sit together and decide what to do.
Rory Beattie
11:26:57
Well, Joe also, just for the record, Joe hasn't used any design system components either, it's just Chakra 3, so…
Kris Mokrzycki
11:27:02
Exactly.
Joe Worsfold
11:27:03
Oh, no, it was, but I have now got designs.
Rory Beattie
11:27:06
Oh, right, okay.
Well then, I actually think then if that is the case, and again, this sounds horrible coming out of my mouth, but I think using the design system adds another level of complexity because I don't think you guys use the design system at all yet. And I think it's a big unknown how that interacts with.
Joe Worsfold
11:27:10
Light touch.
Rory Beattie
11:27:25
all previous iterations of CSS cascading, as well as design systems that interact with each other, and it's a nightmare.
John Trahearn
11:27:32
It will almost certainly cause us headaches.
Rory Beattie
11:27:35
Yeah.
So, although it's, on paper, the wrong answer, I think, in reality, the right answer is not to use the design system for this particular thing at this stage.
Feels horrible saying it.
Joe Worsfold
11:27:49
Let's… do we want to just… I guess it makes sense to do a… I guess on your side, or a spike to…
Because there's also, there's always options, right? Like, even though it won't be great user journey, depending on how the UX looks, we could always, instead of using a widget, bounce them to a party page without them, like, depending on how much they check their URL, like, and then bounce them back.
But, you know, we have options, so it's… this bike, I guess, is to figure out what's the best.
What's the best way to do it?
Um, but I'm happy to provide very, like… there's no difficulty for me, probably, to just provide much more…
like agnostic components to do the same thing.
But yeah, whatever, whatever we think is best. Also, you could just go direct to API if you want to build some of those parts, things on your side. But then I thought that was some useful.
Another thing for us to provide you is the components for using the search and creates and stuff.
Kris Mokrzycki
11:28:46
I think the component way that gets provided is the right way forward, right? So which, because we don't want to own it. However, uh, yeah, there's a, there's a contract in between what comes out, what we need, but I think that's as far as INRIX should go.
And question whether you don't have enough capacity, whether we have enough capacity, but it should sit outside of in risk.
Joe Worsfold
11:29:12
Yeah, I do think so as well.
Rory Beattie
11:29:12
Yep.
Joe Worsfold
11:29:15
But it's just then, yeah, we can find a way to provide you with something that at least works of interest today.
It would just be a thing of, like, how long.
the party need to maintain two separate component libraries for in risk versus I guess grant on like chakra three design system stuff.
Rory Beattie
11:29:36
Mmhm.
Joe Worsfold
11:29:40
But that would be a good problem to have. Probably if my problem is just I have to maintain two component libraries, that's easier than the.
Current thing of like.
do all this migration. So I'm not against it, it's just to make a choice.
Suzanna Whitefield
11:29:54
And just for high volume, they're not gonna be needing a widget, are they, at all? No, they're just gonna be using via API.
Joe Worsfold
11:30:00
No, I did…
Did ask. Yeah, they'll go just to API. I did say, like, we could obviously do it, as long as it's secure, but I think no one wants to answer those questions yet, so we'll just API it.
Rory Beattie
11:30:12
Mm-hmm.
Okay.
So, it sounds like for Phase 1, then…
Party tagging is something that we need to address.
we have an answer for that, but it's basically using the existing solution, adding in those extra fields, again, to the existing side on InRisk, and Joe, you might need to do something on the… on the widget side, if it's a different widget, we'll find out.
I guess all of that stuff, right? On the feature tagging, we're going to ignore that for phase one, um, and what we're going to do is you're just not going to switch off the backend as it exists currently, so that they can pull through that static list.
Joe Worsfold
11:30:42
Yes.
Rory Beattie
11:30:55
of tags into InRisk still, but we'll look at how we eradicate that in a subsequent phase, right, whatever phase that falls into.
um…
And that might fall into in-risk engine, or it might fall into a change that happens in in-risk specifically.
Probably the latter, I would hope.
Joe Worsfold
11:31:13
Yeah.
Rory Beattie
11:31:15
Cool, alright, so I think that addresses that. I think…
John, the other areas that you covered off before…
kind of give us our answer for in-risk for Phase 1, right? For everything else that's outstanding, that we're aware of, at least, anyway, right? Um…
John Trahearn
11:31:27
Yep.
Yep.
Rory Beattie
11:31:32
And any subsequent open questions might come from tomorrow's high-level, um, architecture session on the in-risk side, which you guys are going to attend, and we'll get Billy to go to.
John Trahearn
11:31:43
Yep.
Rory Beattie
11:31:43
Um… Is there anything else…
that we haven't talked about.
Or is it just the unknown unknowns that we're at.
Sergiu Postolachi
11:31:53
Do we have the correspondent items for, uh…
Version ID… the new fields, I mean.
In our…
Graph project.
Joe Worsfold
11:32:05
Yeah. So I think.
we'll have it… there's just a conversation that I need to have with you guys when you start using the new… the new widget, or the new search, will just be…
Uh, do I have… am I giving back all the same data that you need in my response? But that's not a problem, I can just change it to have whatever is needed, because it's just OpenSearch is indexed slightly differently to how it's stored in.
In Dynamo. Right.
But I think this will get into that element of like when we do it or do the initial kind of spike for it will.
Rory Beattie
11:32:29
Cool.
Joe Worsfold
11:32:34
Well…
Make some changes, but everything's there, like, the data's all there.
I think the only other one that's on my mind is the sanction thing. It's probably slightly to one side, but.
I am aware of work that Boomi are doing, and I think somewhere along the line, we have to unpick the sanctions processing, because it's kind of a mess. Uh… because I've been talking to Boomi, they, like… they sit on top of our events, which… our events fire every time a party changes, or anything changes in InRisk, even if it's completely irrelevant to sanctions.
And they're building, like, an entire, like, cash and item potency checking system.
To double check if they need to resend the sanctions, but what they're basically doing is taking my event.
with party information, then they call the InRisk API because they don't have enough information about the submission, but one at a time because they don't have an endpoint in InRisk that's.
Like, get multiple submissions per, like, a list of IDs, apparently. That's what they told me. And then they reconstruct stuff on Boomi. So Boomi's got all this logic for sanctions that I think we're going to, like, proxy those events initially, but over time we'll want to, like, not just fire them always completely unnecessarily.
And it feels like it's separate, but it's also not separate. Right. So it's kind of a. I just want to flag it here. I think we need a better way to deal with this in the future.
Kris Mokrzycki
11:33:40
A drop, yeah.
Yeah, keep us in the loop on that because it only caused us so much trouble in the past. You know, that sanctions and firing, you know, bulk, fake, let's say false positive changes that are having cascading effect on us.
Joe Worsfold
11:33:51
But…
Yeah.
Kris Mokrzycki
11:34:07
So, uh, whenever we're talking about that, I think we would like to be involved, because the blast radius of that activity is quite significant.
Joe Worsfold
11:34:07
Yep.
Yeah, agreed. And I don't like, I don't love the idea of having all this, what is probably very important logic on the Boomi side that none of us can see.
Um, and I don't know how we're validating at the moment, like, whether or not they're sending it, yes, correctly.
Rory Beattie
11:34:29
Is that Boomi acting as an intermediary between…
Joe Worsfold
11:34:29
Um…
Rory Beattie
11:34:34
party, in-risk, and NTT, the application NTT. Yeah, yeah.
Joe Worsfold
11:34:37
Yeah, yeah, I assume it was born originally from the idea that Boomi will be the middleman between us and NTT, but then they became and have clearly absorbed logic. And I think it goes back to the original thing of, like.
Rory Beattie
11:34:39
Whoa.
Joe Worsfold
11:34:51
Det.
Det.
Rory Beattie
11:35:32
It's definitely in the wrong place, right?
Joe Worsfold
11:36:01
So, like, it feels like we don't have to have to fix it now. But I know, as Chris said, it's already a pain point. So.
Rory Beattie
11:36:01
Yeah, I…
It almost feels like, and I don't know whether this is your thought, but it almost feels like we need to pull out the sanctions logic into its own service that is held independently and we work out who the owner of that is later, right? Because it interacts with multiples of our apps.
Joe Worsfold
11:36:10
Like, when we deliver…
Rory Beattie
11:36:26
beyond our boundary. Um, and then if any of that logic changes, we're kind of masters of it, and it's not asking Boomi to do something that their application isn't fit to do.
John Trahearn
11:36:27
Yep.
Alec?
Yeah, it feels like that Boomi are sort of trying to do that, but it's probably not very optimal.
Joe Worsfold
11:36:37
Yeah.
Rory Beattie
11:36:37
Uh, which we've heard many times.
It's not the right place, is it? Let's be totally honest.
Joe Worsfold
11:36:44
No, it's not.
Rory Beattie
11:36:46
Interesting.
Joe Worsfold
11:36:46
Yeah, it's one to unpick, but I agree. I feel like it even needs to be its own domain or we rationalize which.
domain it should be in, because I don't want Party to have loads of submission data. I'm happy for InRisk to have.
Rory Beattie
11:36:57
Good night.
Joe Worsfold
11:37:00
party data, but I also don't want to then make INRIS feel responsible for sanctions, because it feels like a…
I know something that needs to be worked out.
Rory Beattie
11:37:08
Hmm… hmm…
Yeah, okay. We need to throw on board on that. Feels like a separate problem completely, though, doesn't it? It's, uh…
Joe Worsfold
11:37:14
It does, but it sort of… I knew it was a problem that needed to be resolved, but then when I spoke to Boomi, because they had an ask about this sort of, like, don'.
Scrubbing checks and are like.
problem with not being able to absorb that much data. And I suddenly think to them, I was like, this makes no sense. Um…
John Trahearn
11:37:31
We need to.
Rory Beattie
11:37:31
Suzanne, can you and I chat to Andrea about this? Because I think we need to just…
Joe Worsfold
11:37:32
That you have all this logic on your.
Rory Beattie
11:37:38
You know, Joe's got plenty on his plate currently, and I feel as though this is kind of a… one of those things that we need to elevate.
to make people aware of.
Suzanna Whitefield
11:37:49
Yeah, and also, I found out today that Audit's eyes are going to be on sanctions this year, so…
Joe Worsfold
11:37:54
Oh, there's gonna be…
Rory Beattie
11:37:56
Yes, we don't want it to live in here, then. That's not good, yeah.
Suzanna Whitefield
11:37:57
Probably.
Joe Worsfold
11:37:58
That would be an absolute nightmare, I reckon. But I think, I mean, if Chris with a K was here, I know he got slightly annoyed recently, but I'm still an advocate saying I don't see the benefit of Boomi in lots of different places that we currently have it.
And I think that's one of the… it's a longer-term discussion, I know, but we have to unpick sanctions, at least. I don't want lots of logic being on their side that we can't see.
Rory Beattie
11:38:15
Okay.
Joe Worsfold
11:38:23
Um…
If I'm honest.
Rory Beattie
11:38:26
Okay, so that's… we'll pick that up then, Joe. Um… can I just touch upon one other subject, just very quickly? And that is the…
the go-live cutover of phase one.
So just so I'm totally clear on this.
We can't run MDM and the old world in parallel, right?
As in, for this function, we're gonna run it for feature tagging, but we can't run it for both sets of functionality, uh, in terms of actual party retrieval.
Joe Worsfold
11:38:56
Yeah, I think.
Rory Beattie
11:38:59
Simultaneously, right?
Joe Worsfold
11:39:00
Yes, search, or it's more specifically, like, create, I think, needs to be a single route, because I think it's going to get really complicated otherwise. It's basically, we're designing a mapper to map.
Rory Beattie
11:39:07
I agree with that.
Joe Worsfold
11:39:11
from MDM into the old events, but if we did both at the same time, we'd end up having to have a mapper.
going the other way, and I just think we'll get into a data problem mess of, like, not knowing. I think, obviously, we can… when we do the switch, we can obviously revert, and I think that that'll be fine, because we can actually also…
create stuff in the old graph if we need from MDM.
Uh, I guess for an initial switchover period, but ideally, ideally we not have to…
To do both.
Rory Beattie
11:39:38
Yeah, okay, so that then leads me to the question of, it sounds a bit like the actual release, certainly within RISC, is going to be somewhat of a big bang sort of thing. Even if, even if it gets feature tagged, it's still a cutover exercise, right? It's like they're using one one day and not using it the next.
John Trahearn
11:39:52
Yeah, I…
Good.
And it sounds like it's got to be orchestrated with parties, migration or whatever. So yeah.
Rory Beattie
11:39:58
Exactly.
Yeah, it's…
Joe Worsfold
11:40:01
Yeah, we need to backfill and do some other stuff, but.
Rory Beattie
11:40:05
And then that leads us to the whole thing of, well, getting the test.
scenarios lined up and a test environment that INRIS can use.
is a little bit more involved than perhaps I'd previously assumed.
John Trahearn
11:40:19
But does it have an impact? Obviously, it's a big bang cut over in prod, but in non-prod environments, surely they can be kind of pretty…
Rory Beattie
11:40:29
Yeah, that's true.
John Trahearn
11:40:29
running in parallel.
Rory Beattie
11:40:30
Yeah, especially if it's just the new… if it's the Create, as you've said, Joe.
It's like, if we miss a few parties in the old world in test, it doesn't really matter, right? Um…
John Trahearn
11:40:39
Yeah.
Joe Worsfold
11:40:41
I think as long as we… I mean, on our side, I mean…
It all just comes down to, like, time, as it always does, time and resource, but…
Rory Beattie
11:40:49
Mm.
Joe Worsfold
11:40:50
Even for the non-prods, if we… if we want… basically for prod, if we want to be able to revert successfully without having lost data.
we'd still need to be, essentially, even when MDM takes over, be writing to the old graph, so that if you switch back, that you'd be able to find stuff that was only just created in MDM.
If we do that well enough in advance and we have the time to do it, then the non-prods are not a problem because it would be easy to switch backwards and forwards and both will have the data.
but.
Rory Beattie
11:41:18
Yep.
Joe Worsfold
11:41:19
Um, yeah, I think it is just a…
It's more of a time thing. Like, we have more time, I would say.
We'll do all of that first, get that, like, bulletproof.
Which is a struggle at the moment, because I'm obviously getting pressure from business to still update the old party with, like, new roles and stuff.
So my cutover gets kind of sort of broken up by having to…
do things slightly differently, but, like, we could do it, but I think it's just a choice of, like, where to put the effort. Like, in my… in my dream world, it's just… I've got to do it all by September, and if we're optimistic that things will just work, or as Billy would say, it should just work, then we put less effort into.
I don't know, it's a choice.
Um, so…
Rory Beattie
11:41:59
Yeah, I mean…
Joe Worsfold
11:42:00
I think I'll once I've got. Yeah.
Rory Beattie
11:42:03
I was gonna say, the worry for me is, it sounds like now.
Realistically.
to be comfortable with this, we might have to do Phase 1 as a…
In risk only thing ahead of September 1st, make sure it works and then it's something that high volume utilize, right? They'll, they don't exist in a pre phase one world anyway, right? So that's not the end of the world, but it's almost like something that we need to go live with.
and then high volume turn on later. Um…
Joe Worsfold
11:42:37
Yeah, that, I think that's easier than the reverse because obviously I've said in the past, like if we thought this would be the difficult part, then I could just use MDM for.
high volume. It wouldn't be great for the data option because they'd have to go to two different systems for different curation needs. But.
Rory Beattie
11:42:53
Yep.
Joe Worsfold
11:42:53
I feel like that would be just great. We just move the migration problem further down and make it more complex than if we do in risk first.
Rory Beattie
11:43:01
Yeah, exactly. I think that's…
Joe Worsfold
11:43:01
um…
Rory Beattie
11:43:03
It's just not an option, really, is it? I think it just gets far too complicated.
Joe Worsfold
11:43:08
Yeah, it does beg the question, which is always my… I guess it's… I know that the systems will be doing things a lot better, and I think we will agree architecture-wise that we have to do this sort of decoupling, but…
users will just be like, I don't like the new UX, or I don't like the new way that search results come out. So, like, having time to do a bit of, like.
a user-centered design or speaking to underwriters or whatever first, I guess. I don't know what that's going to look like.
Rory Beattie
11:43:33
Yeah, I think probably I need to speak to Anna and just get the users warmed to the fact that it's gonna change how it looks pretty soon, and…
Joe Worsfold
11:43:43
Yeah, on that note, they'd recently…
Rory Beattie
11:43:44
Somehow say there's no option in this, but at the same time say, oh yeah, your opinion's valid, and everything else that, uh…
Joe Worsfold
11:43:50
Yeah, and not to add complexity to what was already like many moving parts, but if we did get Anna and those guys involved, I know they'd already been building a sort of alternative to the UX, but it's like, do we want to get their input so they don't feel.
Rory Beattie
11:44:01
Oh.
Uh… I know where you're going with it, but I think, again, that if we start trying to make it pretty at the same time, it's, uh…
Joe Worsfold
11:44:06
You know.
Out of the loop.
Rory Beattie
11:44:18
and move where it's stored within the pages and everything, which is what they want to do. I think that makes things too complicated, um, along with everything else. If it was just that, fair enough.
Joe Worsfold
11:44:21
Yeah.
Rory Beattie
11:44:29
But I think that's the proposal. Do all the complicated stuff first, and then do that bit later, when it's totally decoupled from everything else.
Joe Worsfold
11:44:34
Yeah.
Rory Beattie
11:44:37
Um…
Joe Worsfold
11:44:37
I think, I think, I think we'd get the buy in. Go on. Sorry, S.
Suzanna Whitefield
11:44:38
If…
I was just going to say, thanks, Jay. If we are targeting phase one to just, I say just, to release to in-risk, when provisionally do we think that needs to happen in order to meet the September high volume?
Rory Beattie
11:44:50
Yes.
Suzanna Whitefield
11:44:57
deadline.
Rory Beattie
11:44:59
I don't know. I mean, how long do you need to be sure? How long is a piece of string? It's, I mean, I would say at least a fortnight before.
Just to be live.
Um… but that's minimum, right?
Joe Worsfold
11:45:16
Yeah, two weeks is And I think it comes down to, I mean, what we should be doing is should be.
Fairly simple, right? What we're really talking about is saving an ID and a version number into InRisk and making sure that then feeds through the SNS topic. So it should be simple. My worry is always more just around the…
Rory Beattie
11:45:31
Yes.
Yes.
Joe Worsfold
11:45:34
the user input part. But I think we won't know.
How simple it's supposed to be.
still integrated. I'm obviously worried a bit about the Chakra 3, how we put it into in-risk thing, and there's another part of me, it's like, I do want to do the RBACs, and, like, sending of group stuff through properly, because that's one of the reasons I can get rid of old sessions, is I do all the track.
Rory Beattie
11:45:47
Hmm.
Joe Worsfold
11:45:56
API gateway access for people, but we won't know until… I think the sooner we start working on it, the sooner we'll know, uh…
Suzanna Whitefield
11:45:58
that…
Joe Worsfold
11:46:02
So.
Like, how much road we have.
Suzanna Whitefield
11:46:06
And Joe, did we not say on this call that we de-scoped the implementation of Chakra 3?
Joe Worsfold
11:46:13
Yeah, but then I guess that's the bit where I'll have… if I have to build…
Suzanna Whitefield
11:46:14
Thank you.
Joe Worsfold
11:46:17
like, generic components or something I was sending, that's… it'll still take time, so I need to know.
Suzanna Whitefield
11:46:21
Okay. Okay.
Joe Worsfold
11:46:22
What road is going to be longer or shorter?
Suzanna Whitefield
11:46:23
Thank you.
Okay.
Joe Worsfold
11:46:26
I mean, obviously, cursor nowadays, it will be quick, but, um…
Rory Beattie
11:46:27
It…
I was, I was, I didn't want to use that word, but I was going to say, just ask it to remove the dependency on that design system, but keep the look and.
Joe Worsfold
11:46:31
Thanks.
Rory Beattie
11:46:37
behavior the same, right? But anyway, I'll leave that to you. Um, let's…
John Trahearn
11:46:40
How soon is there, will there be a widget that we can start playing with Joe then? Is there, is it sort of almost immediately available or is that a few?
Joe Worsfold
11:46:49
Yeah, it's there, it's published. Obviously, we can talk after this meeting about the… Yeah, but…
John Trahearn
11:46:51
Right.
Joe Worsfold
11:46:55
That that one is currently available. It's, yeah, the the Chakra v three one. So I mean, like I said, I can just publish another.
John Trahearn
11:47:00
Okay.
Joe Worsfold
11:47:02
Component library that's…
to whatever spec you need it to be, I guess, and then I'll do my best to keep them in line in future, but, um…
John Trahearn
11:47:10
Because just thinking the other consideration is the proxying of requests through the API. Presumably the API.
is… is different.
to the new MDM solution, so we'll… we'll need to just align on that in terms of…
How we integrate the new widget as well, um…
Joe Worsfold
11:47:29
Yeah, exactly. That's the more because I haven't seen any risk for a while, so it's hard for me to.
Rory Beattie
11:47:38
It's…
Joe Worsfold
11:47:38
the…
Rory Beattie
11:47:39
Is MTM stood up on its own cloud infrastructure, Joe?
Joe Worsfold
11:47:43
Yeah, so MDMs will.
I mean, to be honest, it's basically just Lambdas running everything, including the UI, but, um…
Rory Beattie
11:47:51
Sorry, I mean, has it got its own account that it runs on?
Joe Worsfold
11:47:56
Oh, no, it's in the same current.
uh… sort of… you know, we've got 3 accounts with 4 environments, it's in the same… it's in the same world.
Rory Beattie
11:48:00
Yeah.
Yep.
Joe Worsfold
11:48:05
not AWS 2.0 yet, although I would have liked it, but I guess it's not ready to do that anyway.
Rory Beattie
11:48:11
No, okay, so that's, that's fine then. That, that probably removes a complication that I was thinking, because if…
Joe Worsfold
11:48:17
But it should be. It should be easy. But I think it's more the devil's in the detail, more around my own desire to do the.
Sort of.
Access bike.
Rory Beattie
11:48:28
Yeah.
Joe Worsfold
11:48:28
the way I'm doing it for DataOps, and… but I think we just need to… it needs to be discussed. I don't… like I said, I don't know exactly how the…
data's going back and forth between a risk and party via the widget at the moment, because I know you mentioned, like, the query parameters and stuff, but…
I'm hoping there's a more eloquent way we can use it, but it would just come down to, like…
Now it's all wired together.
Rory Beattie
11:48:48
Yep.
Yes.
Joe Worsfold
11:48:52
I mean, ultimately, the widget should just be sort of surfacing what also the SDK can just give you, like, methods to do stuff, but…
It will come down to the components and what hooks there are.
How that works, then, in any risk?
Rory Beattie
11:49:07
Yep.
Okay.
Joe Worsfold
11:49:10
It's probably what I would do, obviously, and again, this is because it's, I don't know how interesting setup, but basically the way I would do it is like you obviously instantiate a version of.
RTSDK, which is then passed to the components, or, like, parts of it can be, like, the methods, and then it's all wired together, but…
It's easy for me to say because I'm not on the in-risk side doing it.
Rory Beattie
11:49:32
Who put together the new widget, Joe? Was that you?
Joe Worsfold
11:49:36
Yep.
Rory Beattie
11:49:36
Yeah. And Billy put is most in the know about the old widget, right?
Joe Worsfold
11:49:41
Yeah, yeah, I'm good. Obviously, Billy's gonna be. Yeah.
Rory Beattie
11:49:42
So, without…
No, sorry, sorry. Without that getting too complicated tomorrow then, John, in the high level.
But we still need probably enough detail for you to take it into a low level, right? You probably need to know what the current mechanism is right now for how you retrieve that data and then whether that stays the same or whether that changes in a brave new world, right? Because that strongly affects your ticket.
John Trahearn
11:50:05
Yep.
Joe Worsfold
11:50:07
Yep.
Rory Beattie
11:50:08
Um, so yeah, that's just something that we need to make sure that we cover off.
Tomorrow.
Joe Worsfold
11:50:15
Yes.
Rory Beattie
11:50:16
Okay.
Fine.
Joe Worsfold
11:50:19
Again, this shouldn't be a complicated thing to do. It's just.
Rory Beattie
11:50:22
No, no, it shouldn't be. It was just what you said there, like about the parameter thing or whatever else. And I know you either weren't necessarily aware of that or have done it the right way or whatever else going forward. So it's probably a different mechanic for how these guys are retrieving that, which John needs to be made aware of.
Joe Worsfold
11:50:22
I'm… I'm picking…
John Trahearn
11:50:37
No.
Rory Beattie
11:50:37
At the very least. Yeah.
Joe Worsfold
11:50:38
Yep.
John Trahearn
11:50:39
Yeah, my assumption was it would be fairly similar, but if it's, you know, if it's different, so be it.
Rory Beattie
11:50:48
Mmhm.
John Trahearn
11:50:48
We just need to know the details.
Rory Beattie
11:50:51
Yeah, and I… yeah, it's just removing that assumption, isn't it? Because that was my assumption as well, but just when Joe said that, I was like, ah, that might not be valid.
John Trahearn
11:50:57
No.
Joe Worsfold
11:50:59
Yeah, I will say that, obviously, we're rebuilding the widget because we need to not have it be designed to track 33, then we can't just rebuild it to be exactly the same sort of pattern as today, and we can…
Park that and come back to it if we want to change it in the future.
Rory Beattie
11:51:12
Yeah. Yeah, I don't think there's any problem in the future, is it? It's just addressing the now, the phase one. Um, one other question then, John, are you super aware of how…
Joe Worsfold
11:51:17
Yep.
Okay.
Rory Beattie
11:51:23
That works on the in-risk side, as in how it retrieves it and everything, so we can have the…
John Trahearn
11:51:28
Probably need to brush up on that for tomorrow.
Rory Beattie
11:51:31
Yeah. Okay. Cool. Cool, cool, cool. Yeah.
Awesome.
Excellent.
Any other? I feel as though we've covered a lot of ground there, so perhaps that's as much as anyone can take. Maybe we catch up again after the high and low levels.
So maybe this time next week. I assume we'll have both of those sessions by that point, right? Um…
And then we can just make sure, yeah, you guys are on track, and you've managed to take it into that sprint whenever you can.
Starting next Wednesday, I think. And then, Joe, you can ask any other questions that you've got that affect your side of the world as well.
Joe Worsfold
11:52:13
Yep.
John Trahearn
11:52:14
Yeah, I mean, we can certainly bring in the prerequisite stuff, hopefully.
Rory Beattie
11:52:17
Yep.
John Trahearn
11:52:18
If it's not the meaty ones next sprint.
Rory Beattie
11:52:21
Yeah, cool.
And maybe we can kick off, um, Joe in the interim, getting together a widget version that John can use, so he can, uh…
do a few tests, or… or start the build in anger on that following Wednesday.
John Trahearn
11:52:36
Yep.
Joe Worsfold
11:52:36
Yeah, I've got… my goal at the moment is… I know that we've got other stuff in the roadmap, but to basically get way more involvement from the rest of the team in MDM, rather than… at the moment, it's supposed to be me, B.
And then Billy, infrequently. But then, the more it's shared, the more knowledge is available, the more easy collaboration's gonna be, and obviously, Billy wants… I've talked to him, and he's gonna…
Basically help me.
with the curation UI, and we could get into that with the widget, so then we're getting all of that nice, uh…
Knowledge between the two.
Included in MDM, however we want to. Um, yeah, should be able to move fast, but it's just been…
Rory Beattie
11:53:12
Yep.
Joe Worsfold
11:53:15
Oh.
blurry so far.
Rory Beattie
11:53:17
No, it's less than easy at the minute. I mean, we've changed everything about the way that code's developed, so uh…
It was bound to be a stumbling block, right?
Joe Worsfold
11:53:26
Yeah, a lot happening, but should be good.
Rory Beattie
11:53:30
Awesome. All right. Do you guys want to get lunch seven minutes earlier than you may have planned to?
Joe Worsfold
11:53:40
Yeah, awesome. I'll come have a quick break.
John Trahearn
11:53:41
Yeah, I've got a session with Joe at 12, actually, so…
Suzanna Whitefield
11:53:41
Hi, everyone.
Rory Beattie
11:53:43
Thanks very much for that, guys. Really, really good.
Suzanna Whitefield
11:53:45
Okay.
John Trahearn
11:53:46
Yeah, yeah.
See you tomorrow.