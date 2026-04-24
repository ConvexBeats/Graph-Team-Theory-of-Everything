Yeah.
Why so long?
Um,
like a party will award agreement came out of the woodworks at four months in.
The vendors.
Unknown
14:00:36
I have words about the language I'm not gonna use.
Speaker 1
14:00:40
And then,
like,
this electrics issue,
and,
like…
searches.
Camden are,
like,
the slowest local search.
And the whole of the UK.
Let's see.
Yeah.
I'm not in a chain.
And I was like,
"Convincing is not a stressful process at all." Oh,
I was there.
I'm so sorry.
Are you sure?
Oh,
God.
Yeah.
Sure.
Strangely today,
partly there.
in next Friday,
packing up my current flat,
which is nice,
Speaker 4
14:01:14
so yeah.
Speaker 1
14:01:15
That's very cool.
We'll be back to say champagne on the rear terrace next week,
I think.
Speaker 4
14:01:20
Yeah.
Speaker 1
14:01:21
So are you renting at the moment?
No,
I actually,
I bought the place that I live in with my sister like 17 years ago.
So I've,
I've sold that to her part of it anyway.
Speaker 2
14:01:35
So yeah.
Speaker 4
14:01:36
Nice.
Speaker 2
14:01:37
Very cool.
Are you going to miss living with your sister?
But we're like… I mean,
I will not miss her mess.
She's like… she's like…
a true artist,
and she's ecstatic.
Like,
everything is just…
chaotic mess,
more chaotic than I am.
So that part of it I won't miss,
but I think,
um…
Yeah.
It'll be weird,
but we're only 10 minutes away,
so… Yeah.
Sharing the dog.
Oh,
nice.
Yeah.
Uh,
yeah,
I lived with my brother for 10 years,
and um…
The first couple of years were fantastic because I didn't have to deal with this.
Unknown
14:02:21
frustrating habits.
And now look back up.
I wish I'd go back to the party house.
Speaker 2
14:02:30
Oh,
really?
Yeah.
Speaker 3
14:02:32
Got it.
Speaker 2
14:02:34
Yeah,
yeah.
There are respective other halves.
Unknown
14:02:42
Never mind.
Hey,
Rory.
Hi,
Rory.
Rory Beattie
14:02:49
Hello.
Unknown
14:03:16
Hold on.
That makes sense.
Oh,
yeah.
Yeah,
no worries.
Speaker 3
14:05:39
Hmm.
Um,
well,
I don't need to,
I'm waiting for it.
You know.
Speaker 4
14:06:15
Yeah,
I agree.
Speaker 3
14:06:51
Okay.
Thank you.
Sure.
Okay.
Okay.
Speaker 4
14:07:17
Yeah,
right.
We'll have to go around these two.
Yeah.
Speaker 3
14:07:36
Yeah,
Speaker 4
14:07:38
I mean,
it's going to be strict.
Unknown
14:07:46
Yeah,
type check fails.
Speaker 4
14:07:49
Yeah.
That's it.
It's like,
are they not there?
Yeah,
it's basically like TypeScript.
We call them an un-TypeScript,
like we should.
Speaker 3
14:08:02
But…
Speaker 5
14:08:09
Okay,
so…
Recap of where we got to then,
Speaker 3
14:08:14
and just different points we've made,
Speaker 5
14:08:16
is that we recognise that for the.
MVP development,
we need to update our first search widget,
PCT,
create the ability to the manual parties by that application or by PCT request.
And you'll also see your grouping and create a proxy for the DU events.
What that does,
it gives the ability that we wouldn't be dual renegative.
MDM and graph in a meaningful way.
Um…
we would move directly to running MDM by itself,
and we wouldn't commit to maintaining graphs.
reliability and data structure in terms of RTS submissions.
For events.
In order for us to do that,
we would require a risk to third-party data.
and.
Submit to us submissions and requirements.
and update the Snsp.
To include the party id,
so that we can tie back order.
If they aren't able to do that in a good time.
then we would have to go back to looking at dual running for MDM graph.
But that is so complicated versus the changes to in risk that…
We would have every push to make that change.
Thus simplifying our approach.
I made a couple of notes about migration in that we would have a two-day crossover for the older new PCT.
But a direct,
straightaway cutover from the event stream from Graph to MDM proxy events.
Um,
then towards an end state,
we would look to…
move where feature tech currently is,
but that would require buying from in risk and dependency on Amazon's work through platform in risk.
And we would also look to change the proxy event into a direct stream from Dynamo.
into the EU,
but that would require dependency on it being able,
ready and happy to consume that stream tomorrow,
if you like.
Speaker 2
14:10:34
Questions?
What about all those endpoints of feeding across the URL?
Then,
Speaker 7
14:10:44
I'll put the task in the chat for us to just review,
and…
Let's see.
What?
Whatever comes out of that,
Unknown
14:10:53
we'll…
Yeah,
Speaker 6
14:10:54
I think if they're in use regularly,
we…
part of the flag migration is,
Speaker 5
14:11:01
we'll run those through to the new Mdm.
Speaker 6
14:11:04
And send it to the Graphql.
Speaker 5
14:11:07
So they'll still work function,
Speaker 6
14:11:09
and no one will know.
The exact same result shape initially,
but if not,
we can just get rid of it.
Speaker 5
14:11:17
Cool,
and we believe they're primarily sanctions-related.
Sanctions and submissions for client data,
Speaker 6
14:11:23
right?
Yeah,
and there was just the GraphQL one as well,
right?
Yeah,
please check that.
Speaker 8
14:11:30
I don't believe that there's anyone who's ready for that.
Unknown
14:11:34
Oh yeah,
that team has up.
He's Jay,
Speaker 7
14:11:37
who's in the Boomi team,
or in the Boomi channel.
Speaker 5
14:11:41
Let me see.
Yeah,
Jay-Z.
Speaker 6
14:11:46
We'll see at some point.
Talk to the DU,
it's whether or not… they obviously use the…
event for everything like they do today.
But if I was at the EU,
I'd probably also just say…
You might as well send us your stream.
We can store it and not use it for anything,
but that's it.
Speaker 7
14:12:24
Die.
Ist.
Test fit Barkie.
Sort of around the corners of it.
So.
I know we've got,
like,
a production-like application already,
but once we can,
we should really get it in front of the actual users as quickly as possible.
And so…
Actually,
we said the 1st of September or whatever.
Yeah,
ideally,
I'd be able to give.
Everyone who is,
Speaker 6
14:13:04
uh…
consumer the packages that I've got.
So they can also get the OpenAPI spec,
or use the actual package that…
it's just an SDK,
but I'd suggest doing this.
Obviously,
I think they're most going to use the widget,
but if they need to,
they can use the SDK.
Whoever else… I don't know who else.
Calling us literal code,
though,
if that's the problem,
right?
And taking the SDK is by far the best thing,
because it's…
You just do.
getParty,
you don't have to worry about that.
some aspects.
Finally,
you have to map the model that we can provide the model like very easily.
I guess.
Let's have the conversation about absorbing that information as well as it.
Yeah,
it is a small subset of people,
Speaker 5
14:13:45
as it seems to be.
Then,
if we're at the conversation.
Yeah.
Albeit,
I don't want to de-risk our delivery in any way,
so it does just mean we're replacing what we have.
Yeah,
yeah,
exactly.
Yeah,
I think we can,
Speaker 6
14:13:58
but I guess you took it as one,
yeah.
when we do finally deprecate and decommission,
we give them plenty of early warning.
This is where I guess it's useful once we've got all those contracts.
Laptop.
Which I think we do,
but,
like,
it's just whoever's using that.
Existing breed endpoints,
or going live building together.
Speaker 4
14:14:21
Okay.
So I want to talk about.
Speaker 5
14:14:28
enriching data.
But there's 1 other thing that sticks out to me,
which is this party versions and revisions piece.
Help me to understand the end state architecture for.
We make a change to party.
How do in risk know about that?
Or do they bear that we may have?
Speaker 6
14:14:49
This is all that keeps coming up when we talk to them where I think the way they do it right now is they don't care.
They just take a snapshot of it.
But there's probably a thing to say that your domain probably should care.
In some cases,
but it does feel like they need their PO to work that one out in terms of…
we can send them all the events without changes,
or,
like,
as a draft becomes active,
or as a sanction status comes through to us,
we can send them all the events.
I just don't know what they will do with them,
because that is their domain choice,
I guess.
Yeah,
so in the entrance,
Speaker 5
14:15:22
they…
There's no real movement for that.
They're still going to store a client name in risk.
They're going to tell us about it.
We're going to store that in risk view.
And that's what they will display to their users.
The search will continue to…
Display our party.
And on renewal,
they'll be asked to reselect parts.
Yeah.
But it's down to them as to whether they care.
Speaker 6
14:15:45
Yeah,
about the changes later.
Yes.
Yeah.
which I'm sure they do now about sanctions,
and obviously we talked about the broker thing,
but that's where we're in a room with them.
Speaker 5
14:15:55
So,
if we are saying that they will care in the future,
are we sending them a version.
of that policy ID.
Speaker 6
14:16:05
Well,
when they when they create it,
let's say it's a new one.
They'll they'll take the id in the version of the version one.
Yeah.
But if it's one that's already in use,
they'll take the id in the current like version might be.
Yeah,
so it's changed,
it's been iterated,
DMV data's been enriched,
Speaker 5
14:16:22
and then,
yeah,
they research for it,
it's now RT123 version 5.
That's always been that thing of,
Speaker 6
14:16:29
like.
my API can support everything in the sense of,
like,
you can call us and go get party by ID and version,
so I give you a specific version back,
or you do get party by ID and a timestamp,
and I give you what the party looked like at that exact time.
Or you could give me the party based on the ID,
and I'll give you the latest.
And then members can decide which one do they want to use.
I think for most cases,
the current thing is they just… it's just as natural,
so they don't ever want to go,
give me the party post and the ID and the version.
Speaker 5
14:16:57
So what would we have to do to enable.
one of those options.
Do we have to say your API is flexible?
True development is flexible,
or…
deterministic from what they ask for.
Deterministic from what they ask for,
Speaker 6
14:17:09
at least from our side of that contract.
What I think is the bit that I keep,
sort of.
Being unsure about which is a business PO decision is,
like,
don't they want to know that I changed?
From creation to,
like,
buying it.
I can show them.
for the difference,
but I don't know if they can,
because at the moment they don't,
but…
Um,
they probably should.
I don't know,
but that,
again,
isn't… I feel like that's me overstepping on… that's.
Speaker 7
14:17:38
I guess something,
some almost a sanction in between.
Yeah,
exactly.
I don't know where that would come in the process,
but I'm sure there's other changes that can happen.
I can send them,
Speaker 6
14:17:48
so it's like they can obviously in a,
I support both.
I support the synchronous world of if they wanted to check.
Did it change on bind?
They could just call me and say.
Here's what it looks like at that version,
here's what it looks like latest.
Is it the same or not?
Right?
They can construct that inside,
or I can provide an endpoint that doesn't compare.
But if we want to just do async,
because obviously we are event-driven,
like,
I can also just,
as soon as I've got my sanction screening result,
and I've got my.
party like approved or revision approved party like set as active.
I send them those events,
and they decide what they want to do with them.
at that point in time.
It's an interesting question for them,
Speaker 5
14:18:27
because the whole sanctions piece is.
interesting.
So sanctions process currently is that a new party is created into our database by Emirates.
We then trigger an event which hits.
Boomi,
which then goes on to NTT to the sanction school.
Then the result is retrieved back or not sanctioned,
and then we update our party with that.
What they'll then do is make a call to us to say what submissions are associated to that party.
We're thinking of firing an event back to in-risk.
with that submission ID saying status not sanctioned,
and then against that submission,
it shows it's not sanctioned.
That feels a very long way round.
to get to a much simpler suggestion,
which is.
Get NTT,
pass it back to the party.
Party passed it to a rescue.
That's much simpler to say,
Speaker 8
14:19:26
"Hey,
you know,
Speaker 5
14:19:28
that party idea that you've attached to all these submissions is not sanctioned,
so I'll have submissions,
but not sanctioned." Yeah.
I'm not saying that's the scope of what we're doing.
But it does enable us to do that in the future.
Very nice.
So just on that update as well,
Speaker 2
14:19:46
you were talking about fine,
but I think there's also an issue with draft.
They're gonna want the graded event as well,
aren't they?
Just in that instance,
perhaps,
I don't know,
but… Okay,
Speaker 5
14:19:58
so that's a really good point.
So they create a…
Party,
and it is a party that is…
Alex Limited.
um…
Party duration,
look at it and say.
Oh,
so it's Alexander Limited.
Yeah.
And that's just an error of putting in the incorrect name.
doing risk,
want to know and change that.
Actually,
it was entered incorrectly.
It should have been correct and properly.
Speaker 6
14:20:26
Yeah,
that is,
I guess,
that is where the between the two of us,
it's like a…
If they put in the name just because they just want to see,
is that name in risk?
I can store it as an alias.
But if then we would actually be like,
no,
your name is wrong.
Yeah,
because we're responsible for that.
That is.
It goes back to the idea of whether or not they're keeping a snapshot at all,
or they just have a reference.
It doesn't matter what becomes more important.
It's like the like merge case where it's.
a completely different party that they tried to create as new.
But we said,
actually,
it's this one.
Yeah.
But in both cases,
like…
They said,
yeah,
unfortunately,
like,
we can support everything,
so we just have to take…
the right thing for us and them with that clear separation of who's responsible for what.
Yeah,
Speaker 5
14:21:14
because in my instance where you've just put in the wrong check is that ideally you would merge that into an existing party and say that one was created in their apple.
It is this one,
yeah.
But you will have a placeholder of whatever that party ID was when you created it,
and when you search for that from us.
You need to go to Alexander Limited,
which is a different party idea.
Yeah,
which again,
Speaker 6
14:21:38
it goes into that like.
I don't know what they want to do in those cases that we have to decide in those gaps.
If they get the events,
then they can update themselves to send it back to the correct ID.
Or if they're storing a draft and it gets merged.
I will support it again.
If you go,
give me the latest for this Id.
It's been merged.
I'll give them back,
I think it was merged into,
because that is the latest,
but they gave me the version of the draft,
and I'll give them that back,
because that was what they asked me for,
the ID and the version at draft point,
versus the latest is now spinning out.
Like,
how they pick between the two,
versus if I send them.
The difference,
they updated it.
It's kind of a…
Because where a party already exists,
Speaker 5
14:22:19
they're going to search for the party ID,
they'll have a name,
and then that name gets stored in their database,
and that's what we'll show.
Where they've created one in error,
again,
they'll create a party ID.
They will store that name.
When,
and if,
do they change that front screen again,
just to say,
you know,
hey,
that part's name is NCS already,
this is the correct curated one.
Yeah,
this is where I think,
Speaker 6
14:22:44
if we ask the English people,
they always sort of go,
we just want it to be as natural,
because they need the…
fact that someone who's looking at that screen,
they wrote a specific thing.
I wouldn't want to just override it without kind of explaining why,
or… Yeah.
Because someone would be like,
what the hell,
like… Which… which is true in the…
Speaker 5
14:23:02
And this is where it gets very confusing or difficult for them is that yes,
you'll have a trading aid.
This company is known as something else within the London market,
or it's a company merged to do a specific business fine.
But then you have the ones that are Ltd versus limited capital versus not apostrophes and spaces that are incorrect,
like Lloyd's didn't get the moment.
So.
I guess it's up to them to define.
I'm not quite sure how they.
create a rule that covers both those use cases exactly as it was entered.
unless it was wrong.
Speaker 6
14:23:42
Yeah,
unless it was wrong.
Speaker 2
14:23:44
Yeah,
like.
But but also for broker.
I think it becomes maybe a bit more complex because of the broken number.
Right?
the broken number is associated with the broken person.
Yeah.
In the company,
so for either one of those changes.
Do you then want to keep the… have the previous version?
Not checked,
you know what I mean?
Yeah.
It's got a regulation of its practice.
Speaker 6
14:24:11
I don't know how they…
claim to be able to support that today.
Because we don't send them those things.
Yeah,
we do.
Speaker 5
14:24:23
Yeah,
and I believe we update them.
For brokers as well.
Speaker 6
14:24:29
Okay,
I think we can do it.
We just need to get them in the room and opinionated about their domain as to what they want for their users,
I guess.
Yep.
Um,
yeah,
Speaker 5
14:24:40
one of the actions I took was need a dedicated break workshop within risk to install it on their side,
what it might need.
and a…
workshop.
Speaker 3
14:24:50
So…
Policy snapshot.
Thank you.
I don't,
Rory Beattie
14:24:56
All of those things, Alex. Just chatting to Andrea yesterday. It'd be good to get Scott involved in that as well, because Scott kind of knows this stuff inside out. Susie, I assume.
Speaker 3
14:24:56
I don't think…
Rory Beattie
14:25:07
You can speak to Scott as well about some of this stuff, but…
Yeah, I don't think anybody knows it quite as well as Scott does, so…
If we could include him in that particular workshop.
Speaker 3
14:25:20
Yeah.
That one is easy to take.
Yes.
Thanks,
Unknown
14:25:29
Rory.
Speaker 5
14:25:32
Okay,
cool.
So I don't think that fundamentally changes what we want to do for party versions or revisions of them saying we're going to install this snapshot is then down to in risk.
how and when they want to receive that,
call our API through whatever version,
in whatever format they want.
Yes.
Okay,
um…
DMV…
So we currently call an Api on.
Greg?
to enrich our data from the Durham Grocery House.
I'm going to assume… fine,
we can continue to do that in the future.
Once a party is created,
we make a call through to that API,
and bring back data,
it's done already,
but you've probably got a placeholder.
Speaker 6
14:26:24
Yeah,
how does it work in the sense of like,
obviously,
the very risk,
especially going through the widget.
It's flow is.
Can they find it in our data?
Yes or no?
Then it's,
can I find it demographically?
Yes or no?
And then if they can't find Dunbar Street,
we create it.
Yeah,
so we create,
Speaker 5
14:26:42
um,
that party,
and that party will have associated Dunbar Street information.
If they found it in Dunbar Street.
Speaker 6
14:26:48
If they found it in Dunbar Street,
like that GFB jam.
Speaker 5
14:26:52
And we also create the ultimate parent start new development and enrich all of that.
Speaker 6
14:26:59
So… And that creates two creation things,
or one?
But no.
It just does it.
It takes it straight through.
Yeah,
we accept that Dunbar Street is the…
Speaker 5
14:27:11
Yeah,
do that.
Where we don't do that is for CUO grouping,
where we can't find a CUO group.
We will then create a curation ticket.
just to do this for your own group.
Okay,
because I do need… I need to map this out in the sense of,
Speaker 6
14:27:28
like…
Going architecture is very,
like…
gated in the sense that,
like,
even if you found it by Dun & Bradstreet,
you'll create the draft party from the Dun & Bradstreet source with all this,
like,
provenance from Dun & Bradstreet,
and create a proposed revision that someone would.
So if I kept that,
this is a heavily gated…
you'd end up having two curation teams,
one for the thing and the parent,
but if we're not,
we can… obviously,
it's fine to not have the gate and skip it.
Just automatically create both.
Yeah.
Um…
But I need to make sure that then I have those,
like,
bits of logic that say when to,
like,
auto-approve.
auto propose approve,
because I still want the history of what the…
the source was from.
with or without a revision.
I guess it doesn't have to have a revision.
Wow.
Yeah,
we could also.
Yeah,
basically,
it's not a skip.
It's like we take it.
It's a bit of a mix for this at the moment.
Speaker 9
14:28:24
So obviously,
everything is in response space.
So.
we've gone to DMV on that sort of phase two of the widget search.
And that's been pulled into the English form.
They send them an event when they hit create requirement.
We don't get that data inherently,
because it is a new node,
because it has a Duns number.
That means it can't have been something that we already in our database and created.
Hence we have to assume that it came back and via the Dmb thing.
So we then do an enrichment call to Dun & Bradstreet,
and then we will merge that data.
So we'll create a DMV node that has all of the… I've got back in the API,
but we'll also merge that data back down on the supply record,
meaning any fields that we couldn't fill out.
from the thing that INRIS gave us,
which would be,
like,
parent on this number,
etc.
We will add into the original party node when we create it to give it a few more details than it had,
but it still prioritized whatever.
selecting the English form,
which should be the exact same as the Api,
because essentially,
we've done the same Api call twice.
We've done it once when we're in the widget,
and we'll have to redo it because enrichment.
Speaker 6
14:29:34
Whereas in our case we'll probably do it once.
Right?
The only thing I would like there.
do the right for the parent as well,
but do we cascade,
like,
do we keep going if it has more parents?
Speaker 9
14:29:46
It's just ultimate parent is the only thing we look at in DMV,
so it's just,
like.
a child parent will only ever be that one,
because it's always just a child has this ultimate parent,
and that's it.
Right,
okay.
Speaker 6
14:29:58
So it's not… So what do I need to store in my party?
This is just ultimate,
not parent ID,
and ultimate parent,
just ultimate parent.
Right,
okay.
Yeah.
And then I have to,
like,
create it,
and then I would see,
like,
in the Dynamo world.
need to just do some stuff around.
Donna Bradshaw is still a unique reference,
so it's… we can't have more than one.
Right.
To Aussie.
Yeah.
um…
Yeah,
Speaker 5
14:30:24
we haven't.
spoken about all those constraints that we put into.
No crochet,
and how we… The vast majority just disappear,
Speaker 6
14:30:33
because a lot of it's flattened into… Yeah,
yeah,
okay.
But…
Dunbrush is one of those where I need to maintain the sort of unique one concern about like,
Speaker 9
14:30:47
I can't amount to stuff.
And in this world where.
Um,
we're maintaining,
sort of,
partner ID and submission stuff in MDM.
at the moment,
if I were to renew a submission,
that generates a whole new client ID.
And that then comes into the graph as a client ID under a party.
There could have been 10 client IDs already for that party.
I now say that here's… it may be the exact same party,
but it's still generating a new client ID,
a new node.
all the submissions that were previously under that requirement now sit under the entirely new client ID.
because of the way in risk for it.
They say this requirement,
and therefore the submissions go against this single new client.
Id.
They don't retain the history of this client.
Id was against that.
It's just all all at once.
That is the only thing,
cardinality-wise,
where I'm like…
what happens in the new world,
because you're gonna say,
when it was created,
here's the client ID,
and it was against this submission,
right?
It was like,
great new submission,
here's this one.
What happens when I renew in response to this new client ID again?
And then,
how do we make sure that all the submissions that we said were under the previous 10 client IDs will come through to the new.
record,
or do we not care?
It's… So… Or do we just… Graph by its nature requires us to say…
Speaker 5
14:32:22
this party with this client ID.
This submission.
Yeah.
Do we care in DynamoDB,
or can we not just say,
this party?
has these client IDs.
They also have these submissions.
I don't need to link those.
Yeah,
I was hoping that we'd get to the point where,
Speaker 6
14:32:45
again,
that we could phase out PartDaddy in the future,
but we need it now,
because the views,
then,
are… the view shape is a bit like…
the system,
so in risk in this case,
because of artificial big firms,
in risk is the system,
and now we have to store this proxy,
which is the client ID,
which is basically what the party ID inversion is.
and the thing that it is attached to,
which I guess is a submission requirement,
or both.
Yeah.
They would keep adding to that as they add them to other submissions.
And eventually we could get rid of the client ID if we can go direct from.
the submission has this party version.
But I don't know if that answers your question,
because I don't know what.
It's,
uh,
it's having the…
Speaker 3
14:33:25
Like…
Speaker 6
14:33:26
Client ID have all of them underneath,
versus just a record of each one by submission by client.
Speaker 5
14:33:34
Depends on…
the API,
actually,
we're looking at,
where it says,
okay,
so it's getting submissions by party.
What does that mean?
Is it simply a party ID submission?
If it is,
then just flatten it,
and we don't care that any risk.
view has this submission that we're creating.
Just keep adding to that list.
Keep adding submissions to a separate list.
We don't care about the connections right now.
The DU don't,
because they link through client ID to…
submissions.
on every side.
So they take the InRisk client ID and map to the submission value in InRisk.
Um,
yeah.
No,
sorry,
that's not really true.
They use the client ID on InRisk side to look at client ID in our table to map party ID,
and then that's how they can cascade all the submissions up.
Yeah,
Speaker 6
14:34:29
because I had in the new thing of just…
an endpoint there.
Given a party idea,
it'll give you all the views it has.
Yeah.
And vice versa,
if you give me a view idea,
I can give you the party that was actually,
I guess,
then I can fulfill,
like,
someone wants to know all the submissions on a party.
I can give it back.
Yeah,
Speaker 5
14:34:48
but I won't be able to tell you that InRisk believes that this submission is attached to this client ID,
and attached to this client ID,
and attached to this client ID.
If you want that information.
That's what I was interested in,
Speaker 9
14:35:01
is like,
is it a…
yeah,
submission of client ID,
or what it sounds like you're saying is anything that you get from,
like,
here's a client ID,
here's some data,
you just pass it on to a record on the party,
and say.
of everything that we know about Renalize,
here is the success of admissions.
And we're not going to tell you which one comes from which,
but just know that if it's coming out of this party record,
it's potentially these submissions.
Speaker 5
14:35:28
Because our…
consumers care that this submission belongs to this party.
They don't care what the reading belongs to.
Exactly.
Speaker 9
14:35:35
And in-risk also care that this submission was linked to this party.
Speaker 5
14:35:38
That's it.
They don't care about the intermediary step.
Yeah.
We do need to talk to Iris again,
Speaker 6
14:35:44
then,
about.
uh… is it API-driven or event-driven,
but if they can attach a view.
We also need to make sure that they detach view from a switcher.
If,
for example,
it's in a draft setting.
And one party used that,
and then came back and edited to be another party.
I need to make sure that I end up doing that.
Only the views that they actually still have,
probably.
Otherwise,
I'm saying events are necessary,
or people need to see us as a source of truth for what is definitely…
during this party,
I have to make sure I'm detaching as much as I am attaching.
Yeah.
I…
Speaker 8
14:36:20
Would probably also like to hold a session with them about.
Speaker 5
14:36:23
anti patterns and data fixes those kind of things where we need to make a change.
What does that do on each side?
Yeah,
because there is a thing where,
Speaker 6
14:36:32
like.
I wanted to support views just for that subscription model,
but…
Again,
really,
the best way to find out is who's viewing party.
It's the system that's got all the parties and what it's attached to,
which is in risk.
But,
like,
I don't know,
for now,
this felt like the most…
replicable,
replaceable way of doing it,
because it currently has all that.
Speaker 9
14:36:59
The only other thing I'll highlight is the resulting mirroring exact.
uh… event schema right now,
I think a requirement of submission sits at a client ID.
Yes.
It's nested in the column ID.
Okay.
Say that again.
Speaker 5
14:37:20
My brain just went like we've got another party.
Speaker 9
14:37:23
You've got your client IDs in the event payloads that we admit at the moment,
I think.
the requirement and submission ID are nested under the client ID.
So it's per client ID,
these are the submissions and requirements we believe are under that client ID.
You are…
Correct.
Speaker 7
14:37:44
Does anyone consume that from us in that way?
Yes,
maybe.
I don't know if they just have our endpoints instead.
Speaker 5
14:37:53
So the sanction thing's really interesting,
because of this event.
Um,
sanctions take name and class ID.
That's it.
Yeah,
because that's where they then call back into it.
I don't think you can do that,
Speaker 9
14:38:09
do you?
Yeah,
do you definitely don't?
What do whom take off?
Speaker 5
14:38:14
Do they…
Speaker 9
14:38:15
requirement ID and submission ID from party,
though,
Speaker 7
14:38:20
or from in-risk event.
Yeah,
because you can ingest in response and party events,
right?
It's from a party event.
As in,
which way?
Speaker 9
14:38:30
To link a party to a requirement,
you take it off the party event.
to create a requirement.
So,
like,
when you're looking for the requirements for mission layer,
that's all coming from English stuff,
but when you're looking to create that singular connection,
what party relates to what business strategy?
Speaker 6
14:38:52
I think for us it'll go back to the conversation we had earlier,
which is…
If we want our model… new model to be fairly pure,
we can do that,
but then we may have to do,
like,
a…
a simple API in Risk Guide so that we can go back and go like,
our fuel doesn't have it,
but we can call it Risk and rebuild that bit.
This is where it's like…
Speaker 9
14:39:12
But there's a lot more.
to us,
because,
like you say,
you could just say,
hey,
I'm making an event about this party,
which has…
500 client IDs underneath it.
I just need a client ID.
What submission and requirement IDs you have for that.
Don't want us to build my own.
Yeah.
Um…
Otherwise,
you get into the position of…
like having to maintain.
If a submission is popped up here on client id 2,
I need to be able to take it off.
I know you want.
Yeah,
which is.
No.
as nice and fun.
Exactly.
Obviously,
Speaker 6
14:39:56
the ideal for your set is.
we get rid of client ID.
How does that pass through?
But,
from the beginning,
so how do we keep the structure the same in that adapter?
I think it's we can solve it either side of the fence of.
As we construct it,
you know,
from what we already know,
or having to go and grab some commit risk.
It's not on the event,
Speaker 3
14:40:20
that's right.
Okay.
Cool.
So that's a really interesting point and.
Speaker 5
14:40:32
Because this is crucial.
So this is an example of a…
Party event,
we admit.
On the pen's right,
I have to save that.
we…
I've found the source system perspective is English client,
and we're providing a client ID.
which is that.
The business key that sits underneath that is a submission reference with that submission ID there.
Um,
we also have an additional requirement ID there,
at the same level.
So…
in our event.
Yes,
we say,
upload in risk client,
and then below that is submission and requirement.
Id.
If we flatten our MVM table like I suggested,
we wouldn't be able to replicate this structure.
Now we're saying that the D you don't fare.
And they don't need it.
Do they care that we're going to change our schema?
Yeah.
Depends on when they try to index,
I don't know,
Speaker 9
14:41:32
I guess.
Because I never tried to extract that nested bit of the JSON schema,
and they wouldn't care.
But I don't know,
because when we said we're going to add requirement,
they said,
yeah,
that's fine,
doesn't matter to us.
I don't know if that's because they don't go far enough nested,
or because they only ever looked at submissions,
and they didn't care about requirements.
If they don't… if they don't have a trial.
in the JSON schema,
they'll never even notice.
If they are currently trying to read the submission in that place,
and suddenly it's not there,
they will go.
We were saying that probably in the views on the MDM,
Speaker 6
14:42:09
we'll take the submission.
ID,
client,
client ID.
Yeah,
Speaker 5
14:42:13
but we wouldn't nest it.
under each other.
So it wouldn't say client ID,
submission ID,
which is how it's represented in the schema.
I could put them on the same row,
Speaker 6
14:42:24
right,
if you would like.
The client ID and the switch ID,
no?
So it's… I know which… not… they're not separate rows,
they're the same row.
Yeah,
that makes sense.
It does.
Speaker 5
14:42:36
But the problem is that would… every time a new,
uh,
image client ID,
those submissions are moved.
Yeah,
that bit I don't get,
Speaker 6
14:42:44
I guess,
but… I mean,
it doesn't make sense,
Speaker 5
14:42:46
that's why it is.
Because isn't it,
like,
Speaker 6
14:42:48
they don't get how they use the client.
That's how they know their submission was looking at that specific party at that point in time,
so they move them on to a later client,
and they've lost that audit history,
which is a math… it sounds like a math problem,
like,
for…
like auditability from like a regulatory perspective,
not just the how does this work perspective.
Yeah,
it's an interesting thing that they would definitely want to change.
Speaker 5
14:43:09
But yeah,
Speaker 9
14:43:09
you can see it because we found this when we were doing the renewal flow is basically we were doing this.
And then they were like,
yeah,
we can't use the current Id that you give us back because we need to get a new one every time.
And you just see.
as you renew each time,
you've got a party with one client ID.
when you have the first submission,
you renew again,
there's now a second,
and it has both submissions underneath it.
The third,
three… Oh,
that's… I feel like that's really bad,
Speaker 6
14:43:37
that then what they've done is gone.
Oh,
because I still need to be able.
all the submissions for a client and move them all onto that client.
Yeah.
Oh,
that was a terrible,
terrible idea.
But for us,
I guess we'll go back to what I think we should do,
which is we saw what we think is correctly being stored when we go to admit this event.
We're going to have to take the client that we have.
and go through in risk,
probably have a really basic API in risk type that just says,
given a client ID,
give me all the submissions.
Then I can construct this row.
Because they're the ones who maintain which submissions are on what client,
and I don't have to care about replicating it as…
moving all the submissions,
because they have that data.
Yeah,
Speaker 5
14:44:17
so rather than creating multiple versions of the same party,
where we've gone,
ah,
previous party had them all against this client ID.
Now,
so it's going to move.
You're not going to follow that.
Speaker 2
14:44:28
But do we need that if we're then storing the party version?
Yeah,
I guess it will.
And say,
which would be the party Id.
And that will.
retrieve all the clients,
and then under that submissions.
That's what I think we we're building towards.
Speaker 6
14:44:45
If in risk can do that.
Speaker 2
14:44:47
If they're if they're surfacing.
they're needing to move all the submission onto client id.
Speaker 6
14:44:51
That's why I feel like we we kind of need them in the room to basically like,
can we not do this anymore?
Because that makes 0 sense to me.
And in a new world you're always tying a submission to a specific party at a point in time.
Yeah.
But because we're stuck using the client ideas of pass through.
that breaks there,
where it's,
like,
creating a client removal subscription.
It's an awkward thing,
because basically,
Speaker 9
14:45:13
I think the way that Bitmo works is a client is attached to a requirement.
So when you renew a submission,
you're actually updating.
the client on the top-level requirement,
which basically then cascades all their joins,
because they then join requirements for submission.
So,
inherently,
they've only changed one record,
which is,
what is the client attached to this requirement?
there's a cascade,
that means every single one of the submissions,
including all the old ones,
are now… Which is where we need to talk to them,
because I would say,
Speaker 6
14:45:43
like,
this is the perfect option,
and we did talk about Xanatom,
it's like…
if we can do a gradual shift towards what he would want in the future,
we should do it now,
because it'll make his life easier.
I would never have put that on the requirement if the submission.
and have a different policy every time,
right?
Like,
it should be on the submission,
not the requirement.
But that is a big ask of Venerisk to change their patent for that and the DU.
And who uses it?
But,
I mean,
100%.
That is what should have been done from day one if the submissions can have different.
RT's at renewal,
right?
Like,
never should have been on the requirement.
Oh.
I guess.
Yeah,
we need their input.
But I think for me it will be.
I'd like to keep Mdm pure about what the future is supposed to be.
But if Inris still does it that way.
In Phase 1.
We'll have to go and ask them.
For that bit,
to construct the event.
To not change anything at the beginning.
Speaker 7
14:46:42
I know it's not an external team,
but just because of the way that they were consuming these events and who…
What's the impact on that?
Are we saying that we won't be able to connect?
a requirement to a party,
then,
who?
If we go with this,
but…
I think maybe in the later phases,
Speaker 6
14:47:02
if they put the party directly on the submission,
not the requirement.
And… we'd have to…
And this is,
I guess it goes back to the point of,
like,
how INRIS was designed.
It's a more complex discussion to have,
I guess.
I think we're fine flattening it,
Speaker 9
14:47:19
because all we're saying is,
this is a requirement,
and it relates to this party.
We don't… that's…
exactly that bit where we're saying we don't care what intermediary client ID that gets me from requirement to party is.
So,
in that case,
in whom we can… The only issue is that currently in our JSON schema.
The requirement is nested under the client,
so we do need to understand.
the tree structure that's there to do the correct nesting.
Whereas,
for whom…
Pym doesn't care about the intermediary piece of that tree,
so you can just flatten it all the way up.
It's just to recreate the event that currently we would still need the message.
It feels doable.
Speaker 6
14:48:03
It just is a this is where again,
like that whole domain condition is like.
We're partly relying on the fact that in risk.
did their domain in a way that I don't agree with.
For how parties work against specific records.
Well,
we can't dictate that to them.
So.
Speaker 5
14:48:22
3 options there.
Um…
InRisk can change the requirement ID process to reconcile party ID rather than creating multiple IDs.
Um,
we could make a call to InRisk to get the submission details and create that nested view when we generate events,
or we update the schema.
Could impact the DU,
but hopefully doesn't.
Yeah,
I think… I just flattened that date.
Speaker 9
14:48:48
Definitely,
I definitely think it's worth asking the DU,
because.
they may not even try index it,
in which case it wouldn't make a difference,
the same as when we added in requirement.
They were like,
no,
don't care about that.
They may currently index it and pull it into our table and our view of party data,
but why are they doing that?
If all they're going to do in the end is join the data from InRisk onto the client ID to get all that downstream information.
Can they just drop the submission field from our table in the DU?
Speaker 5
14:49:23
It's always reliable.
So the…
The reason why I really favor that idea is because.
If we do build in this.
being able to…
Proxy this endpoint.
If interest do come to decide to change it and improve how it works.
then that creates an issue for us,
because we can't then replicate our FEMA,
because they would have changed it online only.
So…
Yeah,
we would create an interim stage that we're happy with,
and when they come along and fix it.
That breaks all.
Interesting.
That made sense in my head.
It is,
that is.
I'm not sure if I explained myself very well,
but… It is,
we're basically on that same topic of…
Speaker 6
14:50:08
Who's responsible for the link?
Because…
I would argue,
should probably be interesting,
because…
They just take our party attached to something,
and they're the one who have that contact.
Yeah.
I've been in that world of,
like.
If we're sending events,
I need to know where they're going,
or someone needs to know.
Again,
they could all be going via the DU,
but like…
it doesn't today,
so… Yeah,
my original thinking when we first discussed this is we would simply pass the party ID to InRisk,
Speaker 5
14:50:38
and then they fill that against submission.
That would be the dream.
Speaker 10
14:50:42
that should join.
Yeah,
ideally.
I think that will be our future status,
Speaker 6
14:50:46
just trying to maintain a non-impact on the DU as our,
kind of.
hard date at the moment,
I guess.
Yeah.
It's just whether or not we pull that in or not,
Speaker 5
14:50:59
I guess.
Let me,
yeah,
let me think of a conversation through the DE team and say,
how are you using my data?
Okay.
And if we can amend our schema to just accommodate.
It would be quite simple to enter us then,
right?
If not.
Hitting you.
How would you feel about?
That's changing our scheme altogether.
I would say,
Speaker 6
14:51:23
specifically for if I was the in-risk PO.
Surely someone has to have an answer for,
like…
Did it.
If you wanted to look at submission.
And what the party was when it was found.
You can't do that because of the way we that feels pretty,
not great from a regulatory perspective.
Right?
I'm sure they could dig it out.
Speaker 4
14:51:46
But like.
I don't know.
Speaker 6
14:51:48
Just in terms of,
like… I think there's a risk,
right?
Like,
what the risk price is very complex,
but,
like…
Shouldn't someone be aware of it?
Yeah,
Speaker 5
14:51:56
I think it is,
because if you look at InRisk,
then their life table will say these…
belong to this card.
Id in their order table.
It will say this submission belongs to this.
Yeah,
because how do they even render like the submission?
Speaker 6
14:52:07
If if they've moved all the submission to the latest client,
I did.
But that now is only as natural of party as it was created at that point in time.
It lost all the.
It's not sure,
unless they can point back to the original client idea it was before it all got put to one.
Speaker 9
14:52:21
Because they don't… it's… the client points to the requirement,
and then the requirement also points to… like,
the requirement is,
like,
the top level…
thing,
I think,
that you join all your data from when you're in Postgres,
and then you would… you'd have a pointer to a client table that says this client has this client.
It also says point it to the submission table,
so this requirement has all these submissions.
So…
when you go to renew the submission.
What we are actually doing is in client world is changing the pointer from the requirements client table to be a different client.
Id.
And then it just cascades.
Then we do all joins.
But all the submissions are now related.
Yeah,
basically,
if I looked at an old submission,
Speaker 6
14:53:03
like,
let's say,
from the literal beginning of COMBEX,
their first submission.
And then I wanted to know.
Who is this for?
Yeah,
Speaker 9
14:53:11
we'll just say… Yeah,
3,
that's a lot.
Speaker 6
14:53:14
Is that not a lot of…
information,
if that person became,
was acquired,
or emerged,
or,
you know,
like… Yeah,
Speaker 7
14:53:22
some things will be,
like…
Updated.
Because even…
you know,
the same… same client on our side,
we could get a data on our side,
and then,
you know,
that's… ultimately,
they're just storing that in their data.
That's right.
It's a new… a new… complete new entity,
every time.
Speaker 6
14:53:40
Yeah,
I just feel I don't mind stale data,
because at least let me know what it looked like at a point in time.
But what I don't like is the idea of lost data.
Which… they can't go back from.
Speaker 3
14:53:52
Like…
Speaker 6
14:53:53
Uh,
because the client would be in the table,
right,
of what it was before,
but now all the switches have been moved to a later one.
How do they know that switch used to be on that old client,
apart from the audit table?
Yeah,
I get what you're saying.
Yeah,
but,
like,
it's fine.
Like,
probably they can get it,
it's just… I think,
if I was in risk,
I'd be like,
yeah,
that doesn't make sense,
we should probably change that.
Anton probably will.
Yeah.
I guess it's not super important.
I just wanted to be like,
if this was Aviva,
I'd have to be like,
if there was a risk that I think exists,
I would have to flag it,
and someone has to,
like,
go away with that,
I guess.
Speaker 7
14:54:26
They've got their new change tracker thing,
just against every.
So,
I assume you went into a submission and renewed it.
it would possibly show on that audit history,
that they've now got out of the UI.
I'm not sure.
Say,
if you renewed a party,
renewed a submission,
changed the party,
I assume that it would say.
Because it's pulled into the…
Speaker 6
14:54:51
Yeah,
there'll be an audit of it somewhere.
I think my.
the risk part is also not just… like,
we could probably find it eventually.
I'm more worried about something along the lines of…
if someone is handling a claim in that switchover,
and then they actually don't understand that at some point it's something that isn't from that original submission.
then they might make a bad choice.
Yeah,
anyway,
that is an interesting problem.
Speaker 2
14:55:17
But that's probably not even in that one thing.
I have a different system which we're not integrated with.
Yeah,
Speaker 6
14:55:23
I guess so,
yeah.
Anyway,
I think,
going all the way back round,
I guess,
just mainly…
We should be able to support the existing event,
but you said you don't need it.
We'll change it.
But I will say,
normally for,
like,
events,
I've…
I get why they said for,
like,
requirement ID,
just add it,
because additive is…
less of a breaking change in removal for these kinds of schemes.
Right?
Speaker 9
14:55:46
So.
Yeah,
so there was a big chat around Renewal's time.
on this exact thing of the fact that client is constant across everything,
every submission on a requirement.
I think Scott did some stuff about that.
He was like,
this is an expected side effect of the way we handle stuff at the moment,
just…
Change it for the most recent submission.
All the other submissions are also going to point like that.
Thanks,
Speaker 5
14:56:12
Bob.
Okay,
we have some options.
Speaker 2
14:56:16
But I think,
again,
that's quite a difficult situation with broker.
Speaker 10
14:56:23
Sanctions for company.
Speaker 5
14:56:25
I think they do.
Yeah.
So broker attaches.
broker doesn't change.
With a broker id,
and they attach that to a submission.
They're attaching.
They're not creating a new one.
Speaker 6
14:56:43
Yeah.
Yeah.
Again,
I guess using the root,
like,
in the…
New World,
it is just as simple as they render what it looked like at that point in time when they're rendering this submission,
or whether they're showing it in risk,
and we can provide that,
or they'll store it from our ideal version.
And then all of this,
kind of,
random,
like,
reattaching,
and then it just goes away,
because we get rid of the…
pass through client ID,
which…
It kind of means nothing.
Speaker 9
14:57:08
No.
It's entirely up to them,
right?
Because they can… they can continue to take the most up-to-date version until something is signed and bound,
and then as soon as it's signed and bound,
lock it to the version ID.
Yeah,
exactly.
So they could do… you know,
they can be…
completely opinionated about what they take on.
Speaker 6
14:57:25
That was the idea,
I think.
Speaker 10
14:57:28
Okay.
Speaker 6
14:57:30
Um,
Speaker 10
14:57:31
sticking off these,
SMP,
um,
Speaker 5
14:57:33
exactly the same as Dun & Bradstreet,
on creation,
calls to SMP,
there's an outstanding question,
whether it's via API or via…
I have it.
Do you?
Which I will investigate.
At the moment,
we just do a manual ingestion of SMP data,
which is not ideal.
Yeah,
we need to change that.
Speaker 6
14:57:57
I think it goes back to the,
like…
We've talked about having a…
a snow pipe like CDC from… but then also that goes back to the original question of…
is party and consumer of the EU data,
or should we actually be the masters of that that we send to the EU?
Either way.
Oh.
Speaker 9
14:58:19
I guess it's one of those awkward ones like DU has always been the SDC,
like the ingestion point of the convex,
I assume.
It's like SDC data comes into the DU and then gets fed up from there.
Yeah,
would be a fairly big shift in the sense of like,
Speaker 7
14:58:34
if we were interested in S.
Speaker 6
14:58:36
And P.
And we were the master of it.
Then we'd always be creating parties to have with the S.
And P.
Data from the point at which we take it.
So we'd have a lot of parties that aren't attached or anything,
because.
We've created them from the data.
But then,
is that not probably what parties should do anyway,
I guess,
because do you have SMP data that…
It hasn't got a party unless it's come to us.
Like I said,
like,
direction of that flow kind of matters,
Speaker 5
14:59:02
right?
Yeah.
So they've got.
SMP data for parties that.
We in party do not know that.
Yeah,
it just feels odd,
Speaker 6
14:59:13
I guess,
again,
unless we don't.
Like,
what are they then used elsewhere,
and does that not work?
What part is it supposed to tongue?
create a golden record that convex doesn't have for a party.
Yeah,
I think… So,
unlike DMV,
Speaker 5
14:59:25
where we make a call based on the fact that we know about the party,
S&P data is… we just got a huge dump of S&P.
company information into a place in the DU.
So it's not targeted,
it is just simply because of your SOP snapshot,
and then we use it at our leisure.
So,
yeah,
different.
Contract arrangement,
I guess,
with SMPBS,
didn't they?
the Api,
Speaker 2
14:59:52
which does entity resolution as well.
I think…
You may want some form of cash with that data.
Instead of having to make over one,
we wouldn't necessarily find the.
answer in the same answer in the S.
And P.
Data universe data dump.
Oh,
yeah.
and not make repeated expenses.
Yeah,
this is where like,
I think,
Speaker 6
15:00:21
ultimately,
if it were me starting from scratch for convex,
I'd say.
make RT the thing that absorbs this information,
creates this,
like,
golden record for them all,
and gives it to the DU,
for everyone else to use.
Because otherwise we get a lot of,
like,
back and forth between us and them,
and…
That feels easier,
but then it means we'll end up in a world where we are…
ingesting source more often,
and creating this process by which we'd probably schedule to update the… Yeah.
Speaker 5
15:00:49
So…
One of the questions I have is,
so this is end state.
What's our entrance date?
Currently,
we ingest SMP manually from the DU.
And that is enough for our current consumers to say,
great,
you've got an SMP number that matches those zones.
Fantastic.
It gets stale,
so we need to do that very regularly,
every few months.
Do we continue to do that until we've got this new platform?
Is that investigated?
Speaker 6
15:01:17
The bit that I want to know to make a strategic choice is how do the DU get the S&P data?
Because actually,
it probably is easier when we're creating all these new parties.
Let's say when we do the backflip from graph into MDM.
If I said,
well,
we always want to know multiple sources of information about that party,
then get them all and then become the source of truth if we then do it that way.
But…
Because it's that thing of…
that's sometimes going to be easier than,
we've now got a party of redundants,
and then now we need to reattach an S&P,
but who's…
Providence,
we believe more,
probably Duns,
but,
like…
Is that easier?
Speaker 3
15:02:02
Earlier?
Um…
Speaker 6
15:02:05
Ah,
yeah,
yeah.
Probably,
but I think it's fine.
I don't think also that you won't want that either,
I guess,
for Phase 1.
We'll just leave it as is,
but…
Um…
We will come back at some point to,
like,
the ingestion of S&P,
and we will have this same conversation,
I guess,
where you go.
Shouldn't we be responsible for that,
and run it on a schedule without us having to manually do backfilling and stuff?
That's fine.
Cool.
In which case,
Speaker 5
15:02:34
we return money and ingestion of S&P data.
for the foreseeable future and make a more strategic call about whether we go by API,
which in our preference.
Depending on that.
for the catching thing.
Yeah,
100%.
Like,
Speaker 6
15:02:49
it makes sense to me because I'm using Dynamo.
My version of the Dun Bradstreet stuff,
and probably Sb.
Eventually is a fairly simple catch where someone could call us and be like.
It's a pass-through where you go if it's within a certain time frame.
We'll just give it back to you.
If it's not,
we'll call dump directory,
update our cache,
and give it back.
Makes sense.
Speaker 10
15:03:13
Excuse me?
Yeah,
thanks.
Thanks for watching.
Maybe I'll get some more.
Oh.
Unknown
15:03:22
for a while,
Speaker 9
15:03:24
and it tastes cool up.
how high the IG discussion is.
say,
I have a client.
Id.
Hello!
Speaker 6
15:03:36
I love this.
Speaker 10
15:03:39
It was like Susie out willing.
Speaker 11
15:03:44
Yeah.
Rory Beattie
15:03:50
Okay.
Speaker 6
15:04:25
Um,
when we do the mergers,
Speaker 4
15:04:27
it will be,
like,
Speaker 6
15:04:29
a choice as to whether or not you view Endpoint.
Like,
if I go…
Move apart,
give me all the views.
Do I go and read?
views from both spines,
like top spines or branches,
or do I,
when they're merged from,
so just chuck all the views on this?
Speaker 9
15:04:45
I guess the save is that you're,
you're only really offering a little relay.
On a,
like,
draft,
like,
an initial…
Speaker 6
15:04:52
I'll probably have to offer it eventually,
just in the case of,
like,
mergers and acquisitions,
which I guess doesn't happen that often,
but…
Yeah,
Speaker 9
15:05:00
I guess I'll be repeat just in terms of like practicing.
The only time we offer,
really,
at the moment,
when you've created a completely fresh record,
because it's manual,
then you need to be able to move it somewhere else,
in which case…
It would be.
I have a draft record with a in rescue.
Therefore all the submissions that I've flattened into the top party records are ones that come with me to the new party record.
If…
Speaker 11
15:05:31
Cool.
Thanks,
Will,
for joining us.
Speaker 5
15:05:35
So,
ultimately,
if I can stab at what you're interested in,
it is…
Um,
in order to,
uh,
be a champion of this change,
but also to understand,
um,
the delivery needs and dependencies from other teams,
we're going to give a further demonstration of the MDM itself.
at a high level,
and then talk about what are those dependencies we have from home delivery.
And across complex.
Speaker 10
15:06:03
Sounds good.
I think so,
yeah,
and then,
yeah,
and then,
like,
what's that mean for,
like,
a really high-level overall roadmap,
I guess,
as well?
Unknown
15:06:10
I don't know if we do just too quick,
Speaker 11
15:06:13
I don't know how easy it is to do all of that,
but yeah,
that's,
uh…
I think we can probably get there.
Speaker 5
15:06:18
Do you want to take the 1st part,
and I'll take second.
Yeah,
do you want me to?
Yeah,
sorry.
Speaker 6
15:06:23
Yeah.
Am I still on the Zoom call?
No.
Speaker 12
15:06:28
Thanks,
Andrew.
Sergiu Postolachi
15:06:31
Hmm. Okay.
Speaker 12
15:06:33
Okay.
Yeah.
Yep.
But it's not all faulty.
That's good.
Sergiu Postolachi
15:06:42
Alex, when you have some time, could you please share the Miro board?
Speaker 12
15:06:47
Uh,
yeah,
thanks.
Speaker 6
15:06:50
Okay,
right.
I'll do a very quick run through,
but obviously stop me if I'm going too fast.
Speaker 11
15:06:55
Can I either make you move around or swap so I can see the screen?
Speaker 12
15:06:59
Or do you assume you also want to see the screen?
Do you actually?
No,
you're not really cat as well.
Speaker 11
15:07:04
For sure.
Thank you.
There we go.
Unknown
15:07:10
Okay,
Speaker 6
15:07:10
so the very quick spiel in terms of both what we do now and what the MDM does is this flow,
which is for at least in risk and high volume user driven where.
Uh,
they need to be able to find the party,
right,
to attach to something.
what that looks like is like 4 different paths to very quickly simplify this flowchart.
It's basically yeah.
So basically discovered,
Speaker 12
15:07:35
they use it.
Speaker 6
15:07:36
We give them an id and a version discovered.
But they believe it's inaccurate.
That's how.
with concern.
So we'll create a variation process,
but they'll still take the id not discovered,
and then they can search another source.
The only one at the moment that's part C,
and then part D is the they couldn't discover in anything.
So we take sort of verbatim from.
the doc,
what it is in creating,
it goes into this revision,
like,
workflow,
which is what curation essentially is.
So we can,
like… there's a detail here as to,
like,
what that looks like in…
the MDM world specifically,
where it's mostly just focused on an API and OpenSearch.
Um…
The revision process then is like ultimately resolution.
So the way I've talked about this,
the reason it's MDM is MDM just stands for master data management.
This is a problem that's existed for.
A very,
very long time.
that when I was at the Home Office,
it was how do I merge multiple data sources about a person,
because I've got passport office information.
these national computer information,
someone might have their first and last name,
sometimes first,
middle name,
last name.
You need to match on,
like,
national insurance number,
because that's a unique identifier for everyone,
blah,
blah,
blah,
like…
Resolving the entity is the problem they're trying to solve with that.
The same as for us is.
Everyone's got a different view of the company,
I have to resolve that down to something.
So.
That's one,
and discoverability of the other,
like,
how easy it is to find it.
So MDM's goal was to simplify and solve those two things,
as you know.
The majority of what that looks like is OpenSearch,
so,
like.
in the new… oh,
this is not going to work,
because I need to update my AWS token,
Speaker 3
15:09:15
so…
Oh.
Speaker 6
15:09:18
So I don't know how much you've doing with the current search,
but ultimately powered by the Neo4j and Cypher queries looking for over all the nodes.
That isn't as,
um…
Alcohol.
Ahí estás suerte.
Por favor.
Es por el coche.
Gracias.
Speaker 3
15:09:49
Y.
Y a.
Speaker 11
15:09:56
Sí.
Speaker 13
15:10:08
search for things in a better way,
and you spell it wrong,
you do this,
it's got fuzzy matching,
it's got various things.
Yeah,
exactly.
So as discoverability improves,
Speaker 6
15:10:17
you spend less time on resolution,
less time on creation,
because if they found it and didn't add it again,
great.
So how that fits into the current flows,
this widget,
or InRisk,
has the exact same flow as it does today,
but powered by a different searching tool,
given a slightly different UI,
because it's slightly different.
And this is an example,
then,
Speaker 13
15:10:38
that would,
like,
from a very specifically talk about,
Speaker 12
15:10:41
like,
what Hugh wants,
right?
This is going to have massive… well,
Speaker 13
15:10:45
the value here is really obvious and really clear,
right,
in terms of it's going to provide huge benefits for them.
they're not paying from you.
Yeah,
yeah,
ultimately,
it will be.
Yeah,
it should.
Speaker 6
15:10:54
Discoverability is good for the users and in risk.
It's also good for high volume mission.
They need to find it.
There's an aside and then like phase two where it's also good for Eva and anyone who needs probabilistic matching.
What about in risk?
Oh,
for this?
Yeah.
Yeah,
just to mention,
Speaker 13
15:11:14
I'm just clarifying that the idea is that this is what,
in response to using the short term,
or is there going to be a bit of a transition where they… This is the,
Speaker 6
15:11:23
so this was what replaces the current search,
but with the same functionality,
just powered by a different backend.
I'm loading all the party details,
all the roles that it has,
and the views and sources and revisions,
everything like clear in one.
in one UI,
if they then want to make a change to a policy,
or obviously another ticket can create a revision from in-risk,
if they said,
this is new,
or unchecked,
disagree with the data,
they can do things like,
in this case.
Um,
they want to try and find it in our data,
so they can do a search.
Uh,
if they find it,
they can basically sort of say,
get all these fields,
and ultimately this is just how we display.
their changes they're making to a part 3.
I think this is just the current curation process,
Speaker 13
15:12:52
but… And this is if they… if they take a new UI that we provide them,
it will include this functionality,
and this will be one of the benefits.
Ultimately,
this,
like,
Speaker 6
15:13:00
this UI is…
taking the functional requirements for the current PCT and putting it on to the new.
Like,
architecture,
from day one.
Uh,
the new architecture handles everything that's kind of there,
and the current piece of the org,
that's what we needed to do,
right?
The functional requirements,
but also is there to enable some…
a future…
state where we do a bit more automation,
right?
Like…
uh,
also approve some of those revisions,
etc,
but basically give them what they need,
as of today,
in one place,
and still have the Jira flow of just getting with Jira.
So,
Speaker 5
15:13:36
as of today,
they're using Jira,
PCT,
and Neo4j to do…
all of this work.
Yeah,
Speaker 13
15:13:42
the benefits for them are going to be improved,
so…
benefits of PCT's bit specifically are going to be improved search functionality,
the ability for them to do the party creation process will be better,
right,
from day one,
if they take the UI.
It will also enable us to make better changes more easily in the future,
we can iterate it,
we can give them extra stuff,
we can involve AI,
do all that stuff.
And is there anything else that's,
like,
a benefit of this?
feature development.
Obviously,
Speaker 6
15:14:10
I think we we can't like getting new features in.
Yeah,
roles,
we'll get all the roles.
Speaker 13
15:14:17
Is that a benefit of the PCT,
though,
or that's a benefit of the whole thing,
right?
that's more benefit of the MDM structure,
or is it specifically that new PCT UI.
And it's probably a bit both,
Speaker 6
15:14:29
or at least the data structures,
but much simpler,
more generic.
You know,
the document store is easier to…
to expand than probably the ontology in Neo4j.
But they can easily add party roles to existing parties through the integrations themselves.
Speaker 5
15:14:43
Yeah.
And just be able to do any that we already support in the backend.
Speaker 6
15:14:48
Yeah.
Which we will aim to do for the…
Speaker 5
15:14:52
party tagging.
Yeah,
we need to set up the schema for the party role,
but they can attach that to any party they like.
Yeah.
I think.
Speaker 6
15:15:00
And then obviously,
like,
Speaker 12
15:15:02
what can they do now?
Speaker 11
15:15:05
Sorry?
Speaker 5
15:15:05
What's the what's the difference there?
They can't create reinsured.
Cover holder claims.
Basically,
they can create road code.
quite a complicated way,
that's it.
Speaker 11
15:15:17
What do they do if they want to,
Speaker 12
15:15:19
if they need to create something else?
Um,
what's the process?
Speaker 5
15:15:23
They have to go to another team in order for them to take it on their behalf,
either through InRisk.
Okay,
yeah.
Speaker 10
15:15:31
Cool.
Speaker 6
15:15:32
Yeah,
so I guess…
The curation… originally we were talking about,
obviously,
having the new architecture,
but using the existing curation UI.
that is actually harder to do than have the new UI sit on the new open API spec and have it generated.
Speaker 13
15:15:47
There's a decision being made already that you're not going to try and use a different… you're gonna… the enhancement is the new UI,
you take it and you don't.
These are the benefits of taking it.
Yeah,
exactly.
Speaker 10
15:15:59
And then I think,
Speaker 6
15:16:01
obviously,
the way that the actual backend is built is specific to this sort of entity resolution flow,
which is probably definitely like a later phase.
But ultimately,
for people like Eva and.
In the future,
where…
the way I always talk about it,
especially with devs,
is,
like.
how we put code into production is like.
It's source control,
it's version control,
and it's peer review,
where you have checks that run,
and then someone approves it.
This is exactly like the Outlook for Data,
where there will be a world where.
If a revision is made,
there's a check that can be run.
If the confidence over a certain amount,
we basically auto-approve it,
right?
So then…
you've got the gates there,
you can have human in the loop,
you can also go past that.
Speaker 13
15:16:47
So these will have those automations on an iterative basis as you go forward?
Yes.
And then,
so,
Speaker 6
15:16:52
once we get to that phase.
We can also do that for the automated flow,
so I'll talk to Josie a bit about how that would work for either.
It would be the same for how the company matching tool currently does it for MGA.
where,
basically,
they just go,
here's the structured data of what we have,
pass it to us,
we give them back in a probabilistic,
like,
matching score that's deterministic,
and then they can… you probably,
if things were to just take the highest percentage,
if it's clear,
and if not,
put a human in to be like… Set whatever thresholds you want.
Yeah,
exactly.
Again,
Speaker 13
15:17:23
those decisions can all be driven by the business,
and what…
people are comfortable.
Yeah,
exactly.
Speaker 6
15:17:28
And then because it's inversion control,
which we don't have in the current system.
We can easily go,
like,
revert,
or…
Go back and forwards,
and make it very easy for a human to audit and go.
is what's changed,
and I guess eventually someone would occasionally want to audit what the threshold is.
If we disagree,
you can revert.
Like,
it just kind of…
For me,
the real benefit,
other than just…
a much,
much simpler architecture.
like developer experience and devops be much less to maintain.
It's also then it's very clear what party does,
what our contracts are with everyone.
And then,
like.
stopped becoming a thing that's slowly absorbing everything,
right?
Yeah.
Yeah,
that's…
So in so in terms of the roadmap.
Speaker 5
15:18:15
So we've been spending today discussing how we go through what we thought would be many iterations of a go live map.
Actually,
today,
it's.
We've essentially worked out that we can.
for PCT and the MDM,
they go live together.
That is,
um,
intrinsic and important that they do so.
If we don't go forward with.
Party curation,
then.
That does leave some gaps in…
how we send information from the existing dynamo to MDM that we would need to figure out.
So…
If they decide not to go forward with the new PCT,
there's going to be some additional work and delay for delivery.
Okay.
Do you have a semi,
Speaker 13
15:19:00
do you have like a view of exactly what that is and what the impact would be?
Speaker 10
15:19:07
TVC.
TVC.
Yeah,
Speaker 5
15:19:11
we would.
need to decouple the manual migration part of our search,
so still send them via Jira tickets,
and then send the DynamoDB.
Um,
as we do now,
into our new FDM,
which is messy.
Rory Beattie
15:19:26
Essentially not an option, Will. It's basically the TLDR.
Speaker 5
15:19:26
Yeah.
Um…
Yeah,
no,
I… yeah,
I was about to say,
just to be clear,
Speaker 13
15:19:32
I get you're not going to do that,
but it's more of the messaging piece around how we raise that.
We maybe don't need to do that with the team.
Yeah,
and even if we do that,
Speaker 5
15:19:43
then in a few months when.
in-risk come along and change their messaging because their database is changing,
then we're also not going to support updating Jira so that it accommodates that either for you,
so…
Yeah,
it's kind of on the old version.
Okay,
Speaker 13
15:20:01
so so big,
big bang,
and go with all of that stuff at one go.
Speaker 5
15:20:09
Yeah,
what's the target for that?
because we think that's the current go live for my volume mission.
But yeah,
so we'll work back from that.
The other… When you say go-live high-volume mission,
Speaker 11
15:20:23
you mean as in,
Speaker 13
15:20:25
like… because the go-live's not till… so the go-live is the start of,
like,
1st of January or whatever,
right?
Uh,
Speaker 5
15:20:30
so when they expect to start the trading business?
Right,
yeah,
okay,
yeah,
Speaker 13
15:20:34
so it's when your dependency… yeah,
just making sure,
okay.
Um,
Speaker 5
15:20:37
okay.
So,
what we do need is… we thought we were going to have to… Sorry,
can I go back to that?
Is that… is that actually a dependency,
Speaker 13
15:20:46
then?
Do you have to have this done by the 1st of September for my volume?
Speaker 12
15:20:50
Because…
Speaker 13
15:20:50
do they really need that in order to do training and stuff?
Is it training,
or is it… is it their test… their test cycle has to have everything fully working by then?
Speaker 5
15:21:00
My understanding is that was into production.
Speaker 14
15:21:02
That's for training.
Speaker 5
15:21:04
Okay.
No,
no,
no,
Speaker 13
15:21:07
if that's… If I'm wrong,
I wouldn't love you guys to,
Speaker 5
15:21:10
you know… I'll do a chat with Sam.
Thank you.
Um,
okay,
cool.
You're probably right,
but I just didn't realize that,
Speaker 13
15:21:18
based on the planners.
Speaker 12
15:21:19
So we… Yeah,
Speaker 5
15:21:20
I was on a call the other day.
Yeah.
Whether that sticks or not is a different question,
Speaker 11
15:21:25
but yeah.
Speaker 13
15:21:26
Absolutely.
Um,
Speaker 11
15:21:26
cool,
Speaker 5
15:21:27
so one of the things we thought we were going to have to do is to consistently update our graph DB with party information.
Um,
which would cause no end of issues,
because essentially they're always going to be out of sync,
and we need to understand how we merge it.
We've realized that rather than just maintaining.
the graph dB to trigger events to the DU,
we should be able to create proxy events.
What's the graph?
Speaker 13
15:21:53
Is this the copy thing?
So rather than going into MDM to graph dB out to Snowflake.
Speaker 5
15:22:00
You can see we've changed it,
so it goes party at MDM into the yellow box in the middle,
which is a proxy event.
into snowflake,
completely avoiding graph dB.
Now we can do that because of some clever thinking from these guys.
But crucially,
if in risk,
change.
essentially three requirements.
So we need Inris to store the party ID in a new table,
which they were happy to do in our call last week,
and I don't think it's much work for them.
Or somewhere,
we're not for us where they store it,
they need to store it.
Number two,
we need them to update their.
messaging to include that party in my day.
Mm-hmm.
And the third thing I've rather forgotten on here.
Speaker 13
15:22:48
It sounds better when it's in group three,
Speaker 12
15:22:52
though.
Broker.
Speaker 5
15:22:54
They need to amend how they retrieve brokers.
From,
um…
That's currently,
because we send it as a URD,
we want to take it directly from research.
Speaker 4
15:23:10
That's there,
Speaker 5
15:23:10
isn't it?
Yeah,
that is it.
No,
I think,
Speaker 6
15:23:14
yeah.
Obviously,
for a high-volume mission,
I'm sure they'll want…
our OpenAPI specs,
and…
Some lower environment ability to kind of test out how it works.
sooner rather than later,
I guess.
I don't know what that would be.
We'd have to work that out from something,
but…
um…
Yeah,
but that is easier in MDM.
Like,
it's already there,
just…
Cool.
That too,
it's like a two-pronged thing,
right?
Change management with data up to hand.
in risk migrations.
happening at sort of the same time,
and validating all of the MDM stuff,
Speaker 5
15:23:50
right?
What that means for us is development,
and then,
um,
I go live to include these items,
so PySearch,
PCT,
um…
the DU proxy events work I talked about,
and a bit of a migration plan for how we will do that.
Ultimately,
no dual running,
except for a two-day cutover,
and a single cutover event stream from Graph to MDM.
And then the end state leaves us with two things outstanding,
which is feature tagging,
which is a weird thing,
standalone,
it would stay standalone until we decide to ask Ingress to do something with it.
And we want to move from the DU receiving these proxy events that we're spoofing into an event stream directly from MDM.
That requires the DU to come on board with us.
So we obviously need to work back and figure out what the dates and effort required for this work would be.
It feels doable within the timeline that we have,
providing we have that buy-in from NRS team to be able to make those changes.
So there's no,
Speaker 13
15:24:51
there's no dependency on DU doing anything as part of this?
Speaker 11
15:24:58
Yeah,
so… Yeah,
Speaker 13
15:25:00
so we've tried to do it so that…
Speaker 5
15:25:06
Obviously,
dependencies on other teams is limited,
which means we're updating some APIs for sanctions,
for…
um,
who,
for anyone else who's a consumer of our data,
easily.
But yeah,
in-risk is the one that we're going to need some actual development support from.
Yeah.
Yeah,
Speaker 14
15:25:22
yeah.
Sounds amazing.
Yeah,
well,
Speaker 6
15:25:25
the thing,
Speaker 14
15:25:26
like…
Speaker 6
15:25:26
Both for desktops,
but also just for our squad.
It's so much less to maintain.
Yeah,
Speaker 13
15:25:34
and that's really important.
Speaker 6
15:25:36
This should be singular.
how it in,
and also then how it in like integrates with it should be similar.
That was part of the problem.
The original thing is,
it's so tightly coupled that.
We were having data quality issues,
and…
Arguably,
I think still in risk,
it's like,
how do they order what the party looked like at specific points in time,
because we didn't capture it.
But…
Yeah,
this.
Without being overconfident,
like,
for me,
it's the…
silver bullet for party,
because it's never quite managed to get to the place that we needed it to be in.
But we can just go…
I'll leave it at SBAU,
right,
Doug?
Speaker 13
15:26:12
Yeah.
Okay,
so the two key things… two key things,
one of which is the length of the conversation you're over here,
is getting…
those guys to agree to this,
and essentially having to move over to the new UI regardless to get the funding conversation they have to move,
and then making sure IMRS can do what they need to do.
How much,
Speaker 14
15:26:34
um…
Is the English stuff,
Speaker 13
15:26:37
like…
basically,
this is a solution,
this is what we need to do,
or how much analysis and unknowns are there around that piece.
So,
Speaker 6
15:26:45
John and Chris already looked at it briefly before.
Rory Beattie
15:26:45
Separate conversations about the interest of, well, it's completely kind of in isolation from this.
Speaker 3
15:26:48
Yep.
Rory Beattie
15:26:52
There are two parts to it. One of them is current in risk, and one of them is in risk engine.
Speaker 3
15:26:53
Yeah.
Speaker 12
15:26:59
Oh,
Rory Beattie
15:26:59
But, um…
Speaker 12
15:26:59
that's… okay,
Speaker 4
15:27:00
I'm full.
Rory Beattie
15:27:00
Both of those are agnostic of each other, but yeah, don't include it in the scope of this because it's kind of already complicated.
Speaker 4
15:27:04
No,
no,
Speaker 13
15:27:05
my question was to understand from that side of things with an in-risk pre-buying hat on,
Rory Beattie
15:27:07
Got it.
Speaker 13
15:27:12
Rory,
where we can follow up.
Rory Beattie
15:27:13
Yeah. No, and all I'm saying is it's not really for these guys to try and work that out on their behalf, right? They've got a big enough job as it is, so we're having that conversation with InRisk themselves, and Joe and I are helping out at the integration, but it is an InRisk problem, yeah.
Speaker 13
15:27:15
What?
Sure.
Yeah,
yeah,
yeah,
that was fun.
Rory Beattie
15:27:30
chat about separately.
Speaker 13
15:27:30
In terms of the requirement,
as in,
in terms of the solution,
in terms of,
we want to pass this,
we want to receive this back,
you've got that,
though,
right?
Yeah,
yeah,
we've done a good job of defining the boundary.
Yeah,
exactly.
For me,
Speaker 6
15:27:44
it sounded like when John Crystal put it.
They want to do it because it is overcomplicated in the currently risk join world.
Rory Beattie
15:27:52
Well, the other reason is, and this is the primary reason why we want to do it here for this as well, and foreign risk, is that one of the whole goals of the year that we talked about in the town halls and everything else is to spread everything out into its own isolated service to do the job it's there to do, rather than the closely coupled.
Speaker 6
15:27:52
So.
Speaker 15
15:27:53
I'm sure of it.
Speaker 6
15:27:57
Thank you.
Speaker 15
15:27:59
If…
Beautiful.
Rory Beattie
15:28:11
set of applications that we have currently. So, this is step one on the in-risk journey for that. Obviously, it's step…
Speaker 3
15:28:11
Okay.
Speaker 15
15:28:14
Yes.
Rory Beattie
15:28:20
N for these guys, potentially. You know, hopefully it's the final step. But, um, for in-risk, it's kind of the first step in getting there, along with there are many others that we need to do to get in-risk to that position as well, right?
Speaker 3
15:28:21
Okay.
Yep.
Speaker 13
15:28:27
Okay.
Bye-bye.
Yep.
Okay.
Thank you.
Thanks.
Thanks for the time.
That was really quick.
So I guess… I think I've got a good view in terms of the benefits and stuff,
so the DQ stuff,
we probably just need to catch up and agree how we message that and what we do on that,
and then the pre-buy stuff,
probably,
I guess,
we'll follow up,
but…
Vielen Dank.
Speaker 6
15:28:58
Ja,
Ich glaube,
ich.
Line.
Ist.
Musik.
Speaker 5
15:29:28
distractors on things that don't matter.
This button is the wrong color,
it should be this shape,
and stuff like that,
which infuriates me when you look at all the other work we have to do,
but…
Yeah,
but we can… this is where we can use Hugh,
because,
like,
Speaker 13
15:29:44
from every conversation with him,
he's entirely like,
don't do that,
don't worry about that stuff,
like… so,
from that perspective,
he will be a helpful… he will help to drive it down if we can give him the…
the overall business case that stacks up,
and ideally some nice,
fancy stuff that you can go and tell people that the DQ team are doing,
whether or not they're doing it,
or you're doing it.
we don't really care,
right?
And they don't really care.
It's a… it's a… they can just… you can message that positive stuff.
Yeah.
Okay.
Cool.
Speaker 14
15:30:16
Thank you very much.
All right.
How much more have you guys…
Finish,
Speaker 12
15:30:20
or are you still stuck in the room for a few months?
Speaker 5
15:30:24
Probably… it depends how long our brains last.
I swear,
Speaker 11
15:30:28
right now.
Speaker 12
15:30:28
You're not joining the cross-product vaccine?
Uh,
Speaker 11
15:30:30
no.
Okay,
Speaker 5
15:30:31
then.
No,
Speaker 13
15:30:32
no,
that's fine.
Right,
bye-bye.
Speaker 15
15:30:33
Cheers,
guys.
Thank you.
Speaker 5
15:30:37
Okie dokie.
So where did that leave us?
So one re there is a du table for one re groupings.
We're going to retain that existing one-read process,
which is they manually do it in PCT today,
so I need to introduce that currently.
What do they do?
Speaker 6
15:30:59
They just go in and say,
this party is this one-read group?
Yeah,
yeah,
yeah.
But what they want to do is…
Speaker 5
15:31:05
Again,
similar to CUO,
based on certain rules,
can we apply who the one-way group should be,
based on this,
enabling the DU?
Okay,
I need to capture all that,
Speaker 6
15:31:14
because obviously the backfill,
it was…
the data,
but I don't have a logic that needs to be when it's created.
Um,
Speaker 5
15:31:23
and then…
Sanctions,
a bit I'm unclear of,
is how we write back sanctions information into the party.
presume.
Boom,
you send us an Api,
and we read the information from that and send it to that rates.
on our event bus.
Well,
they've made an event.
Speaker 9
15:31:43
We forward it to our event bus,
and then consume our own event bus.
So we would just also an Mdm.
Until we sort of.
Directly,
we can just listen to the same event bus,
and we'll consume those right back.
two records brings in that whole question of.
legacy party.
Id uh.
And that kind of thing,
quite readable partially at the moment.
Everything in sanctions is done off that legacy readable party idea.
Is it an issue that we don't start sending them a like,
there's a up level at their current chat.
Dynamo Jack is obviously against the legacy party idea in there.
Where is that?
So…
Um… so that convex password,
yeah?
No,
it's been… sorry.
there.
When I say legacy,
I'm now talking.
Speaker 4
15:32:51
So our our current party,
Speaker 9
15:32:56
Id.
Not the MDM ID,
or you would,
is what currently sits in their Dynamo,
and they check that,
and then there's a pair.
Speaker 10
15:33:09
So you have it done on their side?
Speaker 6
15:33:11
Yes.
Just to say,
Speaker 9
15:33:12
these are the things we have sent checks off for.
So for this party ID and name pair,
we have already sent some checks,
this or not.
And then if they receive an event,
the same party ID,
the name's changed,
they go,
okay,
this isn't… I already built something in the MDM,
Speaker 6
15:33:28
which is basically,
like,
I say in the schema which fields are…
required for a new sanction check,
so I'd only send this sanction.
That trigger,
if…
One of those things changed,
Speaker 9
15:33:40
didn't it?
Every single event will be in there to find which one's which.
Speaker 6
15:33:49
It's… I don't love having them having logic on their side.
I mean,
that is the approach that convex wants to take to it.
Speaker 5
15:33:58
I don't care.
Why did they want me to have?
Speaker 6
15:34:03
Uh…
Basically,
they're doing some sort of item potency or check with logic,
but we could just do that.
And then I just pass you out to NTT.
Um,
because then they're self-reported,
because I've already sent the trigger.
But something changed that requires a new sanction.
So then they're done with the point.
Speaker 9
15:34:23
It's also… I mean,
I guess it's a good question about shape of,
like,
the new events and stuff.
if we're saying that it needs to look the exact same,
then we would want to keep them in the Legacy Party ID at minimum,
where we have Legacy ID.
Although,
Speaker 6
15:34:40
if it's legacy IDs in graph ID,
when I backfill,
I could just have used the same ID for all of them.
I'll teach you.
Speaker 10
15:34:48
Is that the link?
Yes.
Yeah,
yeah.
Yeah,
yeah,
yeah.
Speaker 9
15:34:52
And then you,
you omit that one.
In the event,
and then they'll see it again,
so then…
the same question we had right at the start,
I think,
which is,
do we want to keep like a readable id that is entirely.
unique.
They're basically victims of that like graph party id list.
So it would be unique within that field.
Essentially,
yes.
Well,
I was.
Yeah,
I was thinking about like the an Id.
Speaker 6
15:35:19
That is the system.
Id,
which is the Uuid.
But then also have like a display.
Id,
when the Id is a number of people don't like.
UIDs.
Yeah.
Does that fit?
Speaker 7
15:35:30
So,
Speaker 6
15:35:31
providing that number is distinct.
Speaker 13
15:35:32
Yeah,
I'll make sure that it is possible.
Speaker 5
15:35:35
So,
Speaker 6
15:35:35
we just take graph IDs and put them in display ID,
Speaker 9
15:35:39
and then display ID is what we have in the works.
Speaker 5
15:35:43
And then,
for some reason,
it's legacy.
I'm just,
I'm,
Speaker 9
15:35:47
I'm ready.
Yeah,
okay,
but I think that's where,
Speaker 14
15:35:50
like,
obviously,
Speaker 6
15:35:51
some of the initial work from after today is a lot around just.
The model right now still needs to work.
Don't need to rework that.
And then we need to do 100% backfill or something close to that,
including how now it's going to change the views,
because the views need some stuff that we talked about.
And then…
That process also needs to kind of be like,
okay,
do we just backfill everything and stop,
or do we backfill everything and also create a…
event process that keeps updating the MDM from then on,
until the switchovers,
rather than us losing data.
And then building the adapter.
Basically,
for me,
it's the intercept and the backfill and the projections,
right?
I guess three bits that need to be.
delivered.
Yeah.
For us,
basically.
like,
probably well in advance of us saying we'll do the switch,
because then I'm building my APIs and UI off the top of,
like,
Speaker 14
15:36:52
a lot of that stuff.
Okay,
Speaker 5
15:36:54
um…
So some actions there.
We have…
So we need to get together with the enriched team to talk about what it is we have in store for them,
or would like to.
Do we have any more in risk sessions booked in?
Rory Beattie
15:37:17
Um…
Good question. Not as yet because of the way everything falls.
Speaker 3
15:37:21
Bye.
Okay.
Rory Beattie
15:37:25
Um…
Speaker 14
15:37:26
Yeah,
Speaker 3
15:37:26
sure.
Rory Beattie
15:37:27
So tomorrow, I'm meeting the INRIS guys pretty much all afternoon.
Um, but that's for a different purpose, annoyingly. Um, John isn't in on Fridays, and then all of us go to Romania on Monday.
Speaker 3
15:37:36
Okay.
Yes.
Rory Beattie
15:37:42
So, realistically, the next time that we can chat to those guys is probably next Thursday, if we're not all absolutely exhausted.
Speaker 3
15:37:48
But…
Um…
Speaker 5
15:37:51
Yeah,
I wouldn't be unhappy if it's the following week,
Monday or Tuesday.
Rory Beattie
15:37:56
Yeah, I don't think many people would be. So if I put it in for that following Monday, we make sure we have a decent session to really hone in on what they're after, right? Yeah, cool. All right, I'll add that in right now.
Speaker 5
15:37:57
Yeah.
Yeah,
agreed.
Yeah.
Yep.
Awesome.
Thank you,
mate.
um…
Speaker 6
15:38:12
They're at risk to be able to work in anger when they can.
Again,
initially,
they'll be doing lots of refinement,
but to work in anger,
they need…
The widget,
probably.
Right,
or some mock of it,
but I can give them a widget.
Yeah,
yeah,
yeah,
they would.
But,
I mean,
Speaker 5
15:38:28
they can create the data model behind the scenes to say,
party ID placeholder,
and put that placeholder into your event emission.
that.
Yeah.
It's just…
Drawing it from the widget,
yes,
they're going in the visual.
Speaker 6
15:38:40
But for us to progress on the…
model and back to land projector like the adapter proxy.
That's where I need to start putting stuff into the sprint,
and then decide on,
does that just mostly go to us three for now,
or do I share it out more broadly?
Um…
Because the speed there will help.
No.
Give everyone else a good amount of runway.
When they're dependent on us.
Yeah.
Okay,
yeah,
after.
Yeah,
she might be,
anyway,
I guess.
But yeah,
yeah,
yeah,
Speaker 5
15:39:15
create those tickets.
Put it into those future sprints.
And then.
Yeah,
and sort of validate what we've all just said,
Speaker 6
15:39:24
that this would…
off the graph and probably have again the testers.
I assume we'll want to be able to like have a long checklist of things that work that no one's even looked at yet from a test perspective.
Yeah.
So I need to put them in some.
When?
Why,
in a way.
Because the other thing I need to check,
Speaker 5
15:39:43
then,
is…
how we handle…
Uh,
in-risk client IDs versus submissions,
can we just flatten all of that up to make it simple for us?
Because indeed,
you don't care.
That would be a big win.
If not,
we have to ask any risk to do something else for us,
or we need to store it in degrading.
Cool,
I'll check with Paul Rogers.
Anything else?
Other than,
obviously,
change management,
a great migration approach,
building the whole thing,
testing it.
Yes.
Apart from that.
Speaker 6
15:40:17
Apart from the stuff that I've had to ignore all day.
Speaker 5
15:40:23
Right.
Speaker 6
15:40:25
But the sooner we get the testers involved,
the sooner we know what our,
like,
go-live checklist is.
Yeah.
And as you know,
but for me,
really,
like,
it has come a long way.
But I need to make sure that the data model is,
like…
Yeah.
Got those bits we've talked about today.
Very,
very solid.
Good.
And then the expansion on top of that is just easier.
Hopefully,
Speaker 5
15:40:49
what should be really clear right now is the dependencies and architecture approach we take to building it out.
Details of the data model,
I appreciate.
You will have some,
but… I do have a plan to get the last bits,
Speaker 6
15:41:03
but I think you just need to capture this.
sort of view client ID as a proxy,
but also storage submissions in.
Yeah.
Did you think?
Cool.
And then at which point was there…
Speaker 5
15:41:15
do some more development.
using those tickets that you will create,
at which point I would like us to talk about.
Effort on what we think we're actually going to be able to achieve.
Speaker 6
15:41:28
Yes,
scoping,
basically,
because if we're not careful,
we can just try and do everything,
but a lot of it is not what we already do today,
so we can ignore a lot,
Speaker 5
15:41:37
so…
Yeah,
and it goes without saying where we can make those decisions to say.
It is no better or worse than it is right now.
Yeah,
exactly.
Okay.
Yeah.
I'm definitely out of steam.
I don't know about anybody else,
but…
So that was really good.
Yeah.
Exactly where we ended up.
Speaker 6
15:42:01
Yeah,
especially like 100%.
For me,
it's the.
If all in risk is,
you know,
uses the widget,
and the widget is now responsible for all the creates and updates.
And we can proxy the entire event.
being able to triangle the graph from phase one.
Gives us the runway where we can decommission a bunch of stuff that…
Simplifies our overall workload massively.
Um…
Speaker 15
15:42:30
Okay.
So,
Kristen,
just in…
Speaker 9
15:42:38
In terms of,
like,
platform mapping and stuff,
um…
Basically,
our litmus test is if I can take an event that is currently emitted from PartyGraph,
and I can map its MDM,
and then I can take the MDM record and map it back to the exact same event that PartyGraph would emit.
Go in,
and from the record,
try and map it back to the exact same event in the exact same shape.
we can go into tech,
we can support,
you know,
all downstream consumer stuff,
because they only have consumer events,
and then maybe there's some APIs for sanctions and stuff that we need to rewrite,
but that's,
like,
a couple endpoints.
And then the rest is about interest.
So that's the upstream.
But in terms of the downstream,
we just talked about.
Yeah,
Speaker 5
15:43:29
really good point.
Yeah,
it would be in risk.
And then.
Speaker 6
15:43:35
Yeah,
which I know we've got mapping that's there today,
but I don't know if we need to…
Great,
Speaker 9
15:43:40
a new endpoint that gives us more… The one thing that I found when I was doing these backfills was,
um…
parent stuff isn't that nice,
from the events that currently come,
because all the event says is,
uh,
here's a parent party,
and it just gives the legacy party ID,
and that's all it says.
Which means you would basically have to backfill all your records,
and then once they're in there,
you could then go look up the legacy party ID,
but equally,
you could just run a second batch script that goes through every record,
and if it has a parent party,
looks up the legacy party ID,
and then connects it to the MDM record.
But that's the party idea,
or something.
So,
like,
or you can…
They say you could write a sort of.
Yeah,
Speaker 4
15:44:27
that's all in one go.
Speaker 6
15:44:29
Pick the route that is the simplest.
when we're doing those… because,
like you say,
like,
ultimately,
it's just a… if you have X.
we map,
you get Y,
but that means we should be able to go backwards and go,
from Y,
I can get X,
right?
But just whatever's…
Makes that a simple one-off call would make our lives easier than it being like a whole workflow,
I guess.
Which feels like it should be doable,
it's just we might have to do a bit of work on IT backend if not,
I don't know.
I think that's what I don't see,
like.
We need to.
Go back.
I changed the model based on what we've discussed.
Make that.
Check,
and also then,
yeah,
for the adapter to send all the proxy events,
we need to know,
like.
who's sourced the source of all those bits,
hopefully mostly MDM,
but if not,
we need to get some minerals spent.
Yeah.
Knowing that early helps us tell them what we need,
if they need to add something.
Can our end state be that?
Speaker 5
15:45:28
If the genuine client ID was…
Speaker 3
15:45:31
they…
Speaker 5
15:45:33
for the party associated to a submission.
So once in risk of fill out that,
but they get a direct from risk and we start getting submissions.
Speaker 9
15:45:46
Yeah,
that would be.
Speaker 6
15:45:51
got a end state for who's responsible for it.
how someone gets the event about the… Because that's dependent on interest.
Speaker 5
15:46:00
Um,
and what they choose to do.
It's just that I've got a bug that's been traced here about,
and this client ID's not matching,
and the DUs are in there for Jane.
Yeah,
exactly.
Why don't we have less,
or what?
That's different.
Speaker 9
15:46:16
Yeah,
we don't have one.
What was the problem?
They're different.
Not surprised that we wouldn't have one,
because we're rejecting some special events,
but yeah.
Um,
and it would be different if we had a productive one.
Yeah,
Speaker 6
15:46:30
future status.
Yeah.
Client IDs get completely removed,
because,
I mean,
there is one in a world where you have a party in a version,
right?
Yeah,
but yeah,
I think that'll take a while.
Speaker 9
15:46:40
At the moment,
we're saying that the join happens from the client ID on our record in the DU.
But in the future,
Ingress will be putting out their party ID on there.
in the DU,
in which case DU can join from other sites.
They can join from the NRESC record back to our table.
The moment we provide the lookup reference,
so they can go find it in NRESC,
in the future,
NRESC will provide the lookup reference,
so they can come find it in our table.
Which way?
Which way was it?
Speaker 6
15:47:10
That way around,
because it's there,
the system that's using the party for something.
Yeah.
They have the context.
We don't have to have that in the future.
Speaker 5
15:47:23
Awesome.
Sergio,
Rory,
anything from you guys?
Rory Beattie
15:47:27
Yeah, just one, I think.
In terms of, and this is an unfair question, so I apologize in advance.
Speaker 3
15:47:37
Okay.
Rory Beattie
15:47:38
Um…
In terms of how long we think.
This might take…
What's the gut feel? I appreciate we haven't written it out in a list of actual, these are the things, right? But…
Speaker 3
15:47:53
Business.
Rory Beattie
15:47:53
In terms of good feel.
What we've talked about today.
What, what?
Speaker 3
15:47:58
Oh,
great.
Perfect.
Rory Beattie
15:48:00
How many months? How many weeks?
Speaker 3
15:48:03
How long's a piece of string?
Speaker 10
15:48:06
Hmm.
Rory Beattie
15:48:06
Yeah, I mean… I kind of need to know whether that string's longer than September 1st, but…
Speaker 10
15:48:07
Sorry.
I genuinely like if he…
How many cursor credits I would.
I would say,
yeah,
Speaker 6
15:48:17
I would say,
like,
if we look back.
what is not being my only focus,
but all of the existing model.
and the search and everything stream.
It's all there after only really,
Rory Beattie
15:48:28
Hmm. Okay.
Speaker 6
15:48:29
sort of,
two sprints worth of work.
Rory Beattie
15:48:32
Hmm. Okay.
Speaker 6
15:48:33
Now,
we're fine-tuning it based on this session,
which we would always have to do,
always trying to target it,
but if…
If it was a smaller team focused on it.
And that was their only focus.
I think we can get most of the data model stuff and the proxy and the adapter within two sprints.
Quite reliant on in-risk providers as well.
Yeah,
so that's from our side.
From in-risk side.
I I don't know how long this piece of string is more.
Just what do they actually decide is the right way to do it.
Rory Beattie
15:49:04
Mm-hmm, yeah.
Speaker 6
15:49:05
Yeah,
I have an opinion,
but that doesn't shouldn't matter because their domain.
Um…
And then the other bit,
which is also the string thing,
is the change management.
Like,
I don't know how much of a lead time data ops want to,
like,
train it,
or,
like,
accept it.
Um…
Yeah,
but I don't know.
There's a part of me that thinks like,
if we're all swimming in the same direction,
or working closely together,
it shouldn't take longer than.
you know,
2-3 months,
I'd say.
Like,
what's that?
Rory Beattie
15:49:37
Mmhm.
Speaker 6
15:49:37
6 sprints,
or,
you know… I mean,
there's a little bit of a…
having to align within risk in the weekly release thing might.
I'm sorry.
It depends how flexible they are.
Rory Beattie
15:49:47
I wouldn't… is Interisk that much of a hard dependency, right? Because my understanding would be that you provide them the SDK, and then…
Speaker 6
15:49:56
Yep.
Rory Beattie
15:49:57
as long as they know what that contract looks like, they can kind of develop it their own leisure, so to speak, right? But, like, essentially, we… when everybody's ready, we test it all end-to-end, right? But…
Speaker 14
15:50:03
Yeah.
Rory Beattie
15:50:12
Is that correct?
Speaker 6
15:50:14
Yeah,
I think so.
I think this is where it's like,
the sooner we can…
get our data model into that shape that we've kind of discussed today.
Rory Beattie
15:50:21
Mm-hmm.
Speaker 6
15:50:22
sooner that that SDK is in a position that it's actually,
like.
Very accurate to what they'll get back in terms of the types.
And then,
yeah,
they can run with it.
Rory Beattie
15:50:32
I've got one other question. Sorry.
Speaker 6
15:50:33
Um…
Rory Beattie
15:50:36
SDK…
Is that, uh, are you gonna provide the component level SDK?
Speaker 6
15:50:43
Yeah,
so the way that it's broken up at the moment,
there's 3 packages that the new repo produces.
One is Party Components,
that's irrelevant to most people,
except us.
Rory Beattie
15:50:53
Yep.
Speaker 6
15:50:54
Part of the SDK is all of the data types and the methods to call the API.
and party search,
which is basically the SDK and the components merged into this flow that makes up the search widget,
basically.
So that is the thing they actually need,
assuming they're not going to care so much about the.
Rory Beattie
15:51:13
So are you expecting them to use the actual widget or the SDK with some new component?
Speaker 6
15:51:14
Pulling it as you can.
If I was them,
I would use the search widget and the party SDK,
but I don't use the party SDK to import the data types if they're useful.
But the party search widget will give them back.
Rory Beattie
15:51:28
Well, the other thing is, well, the reason I'm asking that is because we have that other problem, which is that I assume you're writing the party widget in Chakra 3.
Speaker 6
15:51:38
Yes.
Rory Beattie
15:51:39
Which they don't have.
Speaker 6
15:51:40
Yeah.
So I need to,
Rory Beattie
15:51:41
And cannot do. So, which is why I said, if it's just the SDK, I guess they could use some custom componentry for now, and switch the SDK… sorry, to the, uh…
Speaker 6
15:51:43
like…
I… yeah.
Rory Beattie
15:51:53
widget later. There's ways around it, I just think that's something that we need to chat with them about, right? That's, uh, that's a pretty key thing, because that is kind of…
Speaker 14
15:52:01
Yeah,
I would do a… if I was in one,
Speaker 6
15:52:03
it's probably pretty quick to just be like.
is there an easy way to render V.
3 on top of V.
2.
There must be in in a world of like micro front ends that may be more complicated than just me giving them.
The widget has a V2 and a V3.
Rory Beattie
15:52:19
Yeah, that's…
Speaker 6
15:52:19
Because in the cursor world,
it would just try its best to keep them together in line,
I guess,
if you make changes,
Rory Beattie
15:52:25
Well, the other thing is, as well, you could give them a V2 and just say nothing's ever going to change on that V2 ever again, and you will only support the V3 going forward for high volume, et cetera, et cetera, whoever's going to use it. I assume high volume actually aren't using the widget.
Speaker 6
15:52:26
but…
Bye.
Speaker 3
15:52:37
All right.
Okay.
Speaker 6
15:52:41
No,
they're just going to use the API calls via Boomi.
Rory Beattie
15:52:43
Okay.
Yeah, because now that I've said that out loud, that wouldn't make sense.
Okay, so in theory, we could get away with writing it in V2 just to satisfy their need, and then when they're ready.
Speaker 3
15:52:55
Yeah,
thank you.
Rory Beattie
15:52:59
Move up to B3 later as part of another phase, however many phases we're up to.
Speaker 3
15:53:01
Yeah.
Speaker 14
15:53:06
Yes.
Rory Beattie
15:53:07
I assume there's a phase x point x that we're on for that little bit of work.
Speaker 6
15:53:14
Now it's just phase one.
And then once that's quite the triangle of the graph,
Rory Beattie
15:53:16
And everything else.
Okay.
Speaker 6
15:53:21
I'll I'll retire.
Rory Beattie
15:53:22
Yeah, okay, so that's one to add to the dependency list, is just, um, how they, uh, manifest.
Speaker 6
15:53:23
Yeah,
I think.
Rory Beattie
15:53:29
the search, whether it's in the widget, or whether it's via the SDK, or a combination of both, and what chakra version that is on.
Speaker 6
15:53:36
Yeah,
I think ultimately it makes sense for us to still give them a widget,
because that will be the future state,
Rory Beattie
15:53:41
Yep.
Speaker 6
15:53:41
so…
Rather than then maintain one,
but I genuinely would be shocked if there isn't a way to render.
some components.
How do you look?
Speaker 7
15:53:52
Really quickly,
it says that the…
It says… it says that the dependency changed so that two of them were taken away within V3.
And that would be a break and change,
but…
Sure,
if that means it's not relying on two dependencies,
then…
It would be all right,
Rory Beattie
15:54:09
Good.
Speaker 7
15:54:10
but yeah.
Rory Beattie
15:54:10
The problem that they also have, not to get into the detail too much, Joe, but they also have Tailwind in there, which, once you start cascading all of those style sheets and components and things the way that they do, everything steps on each other's toes, so it's not just the V3 versus V2 problem either, unfortunately.
Speaker 7
15:54:11
I think we need to look at it now.
Oh.
Yeah.
Right now.
Rory Beattie
15:54:27
But, um…
Speaker 6
15:54:27
I think we should,
rather than do a V.
2 chakra widget.
We should do it as like Teddy facts,
just most basic version that they can render.
Um,
yeah,
we'll be able to work it out.
Rory Beattie
15:54:38
I'm not actually against that idea, right?
Speaker 6
15:54:40
I mean,
there's an option… they could come,
literally,
to our URL that looks like a risk,
but that's probably more work,
because you don't have a buddy.
URL parameters going backwards and forwards,
which would be a mess,
but… I don't know,
I'm sure… with the smart people that we have,
someone will come up with…
Rory Beattie
15:54:57
Yep.
Speaker 6
15:54:57
But if we have to do a V2,
fine,
Rory Beattie
15:54:59
It…
Speaker 6
15:54:59
we can do a V2.
But I generated the search widget off of the SDK in,
like,
a couple of days,
so…
Rory Beattie
15:55:04
Yep.
Speaker 6
15:55:04
doing a V2 version of it probably is fine.
Rory Beattie
15:55:06
Cool. Well, it's not for today anyway, we need to get them in a room, but it's just one of those prerequisites that we need to discuss with them, so that they are aware of it, and we come up with a plan, right?
Speaker 3
15:55:07
Oh.
Speaker 6
15:55:16
Yes,
Rory Beattie
15:55:17
Let's not try and work it out today without them.
Speaker 3
15:55:17
exactly.
Yeah.
Rory Beattie
15:55:20
Yeah, cool. Okay, um…
Speaker 3
15:55:24
Yeah.
Rory Beattie
15:55:25
Yeah. All right. So I think that gives me a reasonable enough timeline to work with as well.
Speaker 3
15:55:28
Yep.
Rory Beattie
15:55:32
um…
Speaker 6
15:55:33
Yeah,
I guess we should catch up at certain points and be realistic as to,
like,
are we meeting our…
Meeting our goal there,
like…
point in time.
Because our backup is obsolete.
Doing it for high volume only,
Rory Beattie
15:55:47
그러니까.
Speaker 6
15:55:49
not mid risk.
Speaker 15
15:55:52
First.
Thank you for that.
Speaker 3
15:56:05
There we go.
Yes.
Yeah,
Rory Beattie
15:56:18
So, um…
Speaker 3
15:56:18
okay.
Rory Beattie
15:56:20
So the longer it goes on.
Speaker 3
15:56:20
Okay.
Okay.
Rory Beattie
15:56:22
the longer we have to wait to do anything with InRisk Engine as well, right?
Speaker 3
15:56:23
Thank you.
Speaker 6
15:56:28
This goes back to that point,
Rory Beattie
15:56:29
Keep that in the back of your mind. It's not something to add more pressure, it's just something to be aware of, right.
Speaker 6
15:56:30
I guess,
of like.
No,
and it goes,
yeah,
it goes back to that point and we have to figure it out.
is…
Do we think we'd get the stuff that we need done earlier if…
It was just,
like…
a separate small squad,
just getting all of that done,
so that Inris can then take it,
we…
Oh,
yeah,
I don't know.
Rory Beattie
15:56:53
Yeah, well, let's get it all mapped out in terms of what tickets are and the big chunks and stuff, and then we can work out how we divvy up that work amongst a series of people, yeah?
Speaker 6
15:56:53
That's the way this one's now.
Yep.
Speaker 3
15:57:02
Yeah.
Speaker 6
15:57:04
Yeah,
exactly.
Speaker 3
15:57:06
Yeah.
Rory Beattie
15:57:06
Okay.
Great. All right, that is it for me. Sergio, anything from you? I jumped in there.
Sergiu Postolachi
15:57:14
It was a very good session.
would facilitate it by…
Speaker 3
15:57:21
or…
Sergiu Postolachi
15:57:21
Alex.
Speaker 3
15:57:22
Let's see.
Sergiu Postolachi
15:57:23
Congrats for this.
Speaker 3
15:57:23
Let's see.
Rory Beattie
15:57:25
Awesome.
Sergiu Postolachi
15:57:25
somehow…
Speaker 3
15:57:25
I feel absolutely shattered.
Speaker 6
15:57:28
Yeah.
Sergiu Postolachi
15:57:29
Okay.
Speaker 6
15:57:31
Super useful,
though.
And it is… I think this is… I needed this,
because also I felt like too much of this was just in my brain,
and not as,
like,
shared,
but…
Yeah.
I think we will.
We will see the benefit and all like again.
We need a party after we've done this like something.
I don't know.
But let's see how it goes,
first thing is…
Yeah,
the knowing now that we could strangle the graph and proxy the event makes this much simpler.
than where we were even a week ago,
Rory Beattie
15:58:01
Mm-hmm.
Speaker 6
15:58:03
so…
Yeah,
happy days.
Happy days indeed.
Rory Beattie
15:58:09
Awesome. Let's call it there before everybody…
Unknown
15:58:12
Wow.
Let's see.
Rory Beattie
15:58:13
fully fries their brains.
Speaker 6
15:58:14
Yeah,
okay,
thanks.
Everyone.
Yeah,
cheers.
Rory Beattie
15:58:17
Thank you very much for that, guys. Really good work.
Speaker 6
15:58:17
Thank you much.
Appreciate it.
Unknown
15:58:20
No worries.
Sergiu Postolachi
15:58:20
And…
Unknown
15:58:21
Good.
Thanks for that.
Thank you.