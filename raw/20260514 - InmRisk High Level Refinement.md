WEBVTT

1
00:00:04.010 --> 00:00:08.230
Amardeep Badhan: Right, part of your team, or whoever's sharing their screen.

2
00:00:08.230 --> 00:00:19.419
John Trahearn: Yeah, I don't know… Joe, do you want to… I don't know what's the best way to go about this. Joe, do you want to give an intro, or do you want me to just dive straight into, kind of, the stories that we've got? I guess…

3
00:00:19.990 --> 00:00:27.990
Joe Worsfold: I'm happy to give a… give a very quick intro for the guys who've been… been less familiar with this, but ultimately, when I joined Convex, again.

4
00:00:28.180 --> 00:00:34.249
Joe Worsfold: On the graph squad, who's been looking at the party architecture. The main thing that we've been trying to…

5
00:00:34.450 --> 00:00:44.499
Joe Worsfold: resolve, especially going into the future, is decoupling party from in-risk, because they're very tightly coupled at the moment. Ultimately, you've got…

6
00:00:44.590 --> 00:00:58.110
Joe Worsfold: sort of stale snapshots of our data, and we also have stale, or at least eventually consistent, snapshots of your data, and in the… like, historically, it's caused issues with our data quality, either in party.

7
00:00:58.400 --> 00:01:12.309
Joe Worsfold: And we're looking to kind of improve that, but also, for party, we're going to be accepting other applications, they're going to be sending us data, high-volume mission, people are aware of it, and it just… it had to be decoupled, so we've basically done a…

8
00:01:12.490 --> 00:01:27.899
Joe Worsfold: a big piece of work to re-architect Party, and where it integrates with you guys. It should be relatively simple, or we'll find out soon enough how true that is, I guess, but you're obviously using our widgets, and you can call our API to get Party data, which you can store on your side.

9
00:01:28.180 --> 00:01:30.579
Joe Worsfold: The main thing we need is then to store that.

10
00:01:30.720 --> 00:01:50.699
Joe Worsfold: But we won't re-ingest it, you'll store that and a reference to our, sort of, party ID in a version of it. So we'll start versioning our parties in our new architecture. And then, for us, that gives us a bit more flexibility when it comes to when we're ingesting in-risk data, or at least for the moment, taking a composite of in-risk and party data to send for sanctions checking.

11
00:01:50.750 --> 00:02:01.239
Joe Worsfold: It will make our lives a lot easier if you have our reference. And ultimately, the goal is part of the source of truth. MDM literally just stands for Master Data Management.

12
00:02:01.430 --> 00:02:18.949
Joe Worsfold: So in this case, it's… we're mastering the party data, we're managing what they look like over time, how they're curated, and everything, and then you can just trust us to do that, so you won't have to worry so much, or I'm sure you probably don't now, but we'll kind of rationalize all of the entry points that you get party data, both for…

13
00:02:18.980 --> 00:02:23.560
Joe Worsfold: Clients, brokers, party tagging, everything will be.

14
00:02:23.720 --> 00:02:33.569
Joe Worsfold: dealt with by… by us, and we'll provide the components and the other things you need. And I think that's probably where I can hand over to John, and we'll look at these tickets.

15
00:02:35.440 --> 00:02:55.410
John Trahearn: Cool. Thanks, Joe. Yeah, so as Joe mentioned, what we're trying to achieve is, I guess, the least friction way of integrating with the new MDM solution, without, kind of really upending everything else, that sort of is connected downstream from InRisk and potentially relies on.

16
00:02:55.410 --> 00:02:59.679
John Trahearn: Us knowing certain bits of party information and things like that.

17
00:02:59.820 --> 00:03:10.660
John Trahearn: So, we're going with the sort of lightest touches possible, and we believe that to be, essentially augmenting our current data model.

18
00:03:11.030 --> 00:03:21.229
John Trahearn: Which has a couple of tables relevant to this, one of those being the broker table, and the other one being the party table.

19
00:03:21.350 --> 00:03:27.610
John Trahearn: Augmenting those, which are essentially there to store all of the information, there's a snapshot that Joe mentioned.

20
00:03:27.850 --> 00:03:35.270
John Trahearn: augmenting those with two ID columns, right? One of them being, the MDM the new MDM

21
00:03:35.440 --> 00:03:44.110
John Trahearn: party ID and the new MDM party version ID. I don't quite know the full terminology of what that will be just yet, but…

22
00:03:44.570 --> 00:03:47.320
John Trahearn: So this work is around…

23
00:03:47.660 --> 00:03:55.150
John Trahearn: First of all, updating our data model to support storing that.

24
00:03:55.740 --> 00:04:03.510
John Trahearn: And then integrating the new widget, which the party team will provide us.

25
00:04:03.920 --> 00:04:06.619
John Trahearn: And on a feature flag.

26
00:04:07.460 --> 00:04:16.149
John Trahearn: Basically, bringing the new party widget in, to… when a user wants to select a broker, or a client, or do some party tagging.

27
00:04:16.529 --> 00:04:34.939
John Trahearn: And instead of bring the old widget, bring the new widget in, which will, I think, look and feel pretty much the same. It will also be configured to return the same information to us, but with the additional two pieces of information that we've just spoken about. So, that's why hopefully Billy's on the call, to kind of give a bit of a…

28
00:04:34.990 --> 00:04:43.590
John Trahearn: An overview of the original widget, and potentially, kind of, confirm the new widget, and how that might look and feel.

29
00:04:43.730 --> 00:04:44.720
John Trahearn: So…

30
00:04:45.360 --> 00:04:53.000
John Trahearn: So we've got these, five stories that are sort of stemming from… well, first of all, any questions?

31
00:04:56.000 --> 00:05:03.120
Jason Owen: October status, am I… if I'm going off on a, you know, the wrong road, just haul me back in, but, is…

32
00:05:03.120 --> 00:05:10.650
John Trahearn: So status is, is, the… the Lloyd's work, which is… Right, got it.

33
00:05:10.650 --> 00:05:11.540
Jason Owen: Cute, okay.

34
00:05:11.540 --> 00:05:15.470
John Trahearn: Although it's relevant to the party search widget,

35
00:05:15.930 --> 00:05:20.490
John Trahearn: It's… it's relevant more so to the Lloyds 2 work at the moment.

36
00:05:20.490 --> 00:05:32.020
Jason Owen: So the brokers are split into, like, Lloyd's brokers, and these brokers that we're talking about here are, what, retail brokers, or provincial brokers, that kind of thing, or can they sort.

37
00:05:32.020 --> 00:05:32.430
John Trahearn: So…

38
00:05:33.310 --> 00:05:41.739
John Trahearn: So when we… when we select a broker, we launch the party widget, and we pass in a request that says, please give me…

39
00:05:41.740 --> 00:05:56.349
John Trahearn: all the parties relevant to this October status, which is 1984 approved, or hopefully in the new one, it'll be 1987 approved. That still all applies, and I guess it's a good call-out, because obviously in the new party widget, we'll need to do the same

40
00:05:56.360 --> 00:05:57.110
John Trahearn: Thing.

41
00:05:57.390 --> 00:05:57.870
Jason Owen: Right, okay.

42
00:05:57.920 --> 00:05:59.710
John Trahearn: I guess Joe and Billy, that's…

43
00:06:00.440 --> 00:06:04.370
John Trahearn: Presumably a given when we move over to the new widget.

44
00:06:04.370 --> 00:06:17.350
Joe Worsfold: Yeah, on our side, obviously, we're aiming for feature parity, and then also, like, feature enhancements in the future, but yeah, we need the same functionality to exist on our side. It might not right now, but I think that's where we need to…

45
00:06:18.050 --> 00:06:21.399
Joe Worsfold: Like, get all of the, known unknowns out, and…

46
00:06:22.170 --> 00:06:24.610
Joe Worsfold: Sort of get them into the new world.

47
00:06:24.610 --> 00:06:25.619
Jason Owen: With you, thanks.

48
00:06:27.460 --> 00:06:32.639
John Trahearn: Cool. Any other questions before I move on to… What other stories?

49
00:06:34.480 --> 00:06:45.250
Billy Calladine: I've got a question, but you might come on to it, just in terms of, like, parity, from what Joe said. You've got tickets for, integration for clients, brokers, and tagging.

50
00:06:45.620 --> 00:06:49.980
Billy Calladine: Do we know if… Like, the renewals flow.

51
00:06:50.530 --> 00:06:53.510
Billy Calladine: Well, it is client-related, but…

52
00:06:54.310 --> 00:07:01.090
Billy Calladine: is that gonna be… are we just gonna, like, reuse client and being able to renew from client? Do we know how?

53
00:07:01.570 --> 00:07:06.410
Billy Calladine: That's going to be handled, because at the minute, it's sort of… there is a party integration for that.

54
00:07:08.020 --> 00:07:08.400
Kris Mokrzycki: But is it.

55
00:07:08.400 --> 00:07:09.169
John Trahearn: So when you renew…

56
00:07:09.170 --> 00:07:17.769
Kris Mokrzycki: to the existing widget. So, in theory, that doesn't change, because we just need to probably rewire that to the new widget that

57
00:07:18.400 --> 00:07:22.129
Kris Mokrzycki: you will provide, right? It's one-to-one to what we use already.

58
00:07:25.300 --> 00:07:29.770
John Trahearn: That would be our assumption, Billy, that, wherever we are showing the widget now.

59
00:07:30.650 --> 00:07:34.929
John Trahearn: We'd essentially be just calling, show the new widget.

60
00:07:35.500 --> 00:07:39.169
John Trahearn: Which would be… Feature equivalent to what we've got.

61
00:07:40.490 --> 00:07:41.799
Billy Calladine: Okay, cool.

62
00:07:42.140 --> 00:07:47.300
Billy Calladine: And then, last one is, you've got party tagging, but there's another thing called, like, feature tagging.

63
00:07:47.700 --> 00:07:48.650
Billy Calladine: I don't know if that…

64
00:07:48.930 --> 00:07:49.910
John Trahearn: That's funny.

65
00:07:50.160 --> 00:07:50.750
Billy Calladine: Perfect.

66
00:07:50.750 --> 00:07:55.690
John Trahearn: So, we had this conversation yesterday. Feature tagging is, yes, it's driven by party.

67
00:07:56.100 --> 00:08:00.169
John Trahearn: It's not going to be part of this initial phase.

68
00:08:00.490 --> 00:08:04.589
John Trahearn: But it is something that is on the agenda to…

69
00:08:05.190 --> 00:08:12.589
John Trahearn: to kind of, I think, basically reassign, because it doesn't make sense to the party kind of own feature tagging, necessarily, so…

70
00:08:13.740 --> 00:08:14.510
John Trahearn: Yeah.

71
00:08:14.660 --> 00:08:19.299
John Trahearn: It will remain… the old party system will remain to support it.

72
00:08:19.470 --> 00:08:22.260
John Trahearn: Until we get a plan to change it over.

73
00:08:24.580 --> 00:08:26.939
Billy Calladine: Cool, thank you. End of questions.

74
00:08:27.390 --> 00:08:28.429
John Trahearn: of questions.

75
00:08:28.430 --> 00:08:47.810
Kris Mokrzycki: I have one, I have one, maybe not the question, but John, is it worth maybe drafting to everyone the timescale of that? Not the timescale, but actually, there is fairly recent, not recent, but a soon expectation to deliver, I think. Do we have the date when we want to deliver that?

76
00:08:48.470 --> 00:08:54.730
Kris Mokrzycki: Because there was some… there was… there were discussions, like, it's a… Joe, I think you said that there is a kind of… it's not hard…

77
00:08:54.870 --> 00:08:58.540
Kris Mokrzycki: goal, but there is aspirational…

78
00:08:58.960 --> 00:09:03.349
Joe Worsfold: Yeah, I would… I'd probably say the aspirational is sort of…

79
00:09:03.520 --> 00:09:09.300
Joe Worsfold: mid-August, but then, like, we've got a hard deadline for high-volume mission.

80
00:09:09.670 --> 00:09:17.040
Joe Worsfold: well, I don't know how hard it is, but we have a deadline for high-volume mission, which they want to be able to turn on that for September. Now, obviously, like.

81
00:09:17.310 --> 00:09:19.590
Joe Worsfold: They could be calling our new system

82
00:09:19.810 --> 00:09:26.330
Joe Worsfold: first before you guys, but I think, obviously, we've discussed, and I think it makes way more sense that we get in-risk onto MDM first before…

83
00:09:26.620 --> 00:09:29.319
Joe Worsfold: High volume, because otherwise we'll end up supporting two…

84
00:09:29.430 --> 00:09:36.850
Joe Worsfold: architectures at once. So, yeah. Let's… I would say keep, sort of, mid-August in your head, but the big part of that is around…

85
00:09:36.970 --> 00:09:42.670
Joe Worsfold: We obviously need time to validate, test, and confirm some things, before a hard deadline, but…

86
00:09:43.600 --> 00:09:54.970
Kris Mokrzycki: Okay, but it's still distant few… not distant, but yeah, it's not June or July. In my head, I had something like June or July, but yeah, it still gives us enough room for easily implementing that, yeah, thank you.

87
00:09:54.970 --> 00:10:00.360
Joe Worsfold: Yeah, we've got time. I think, ultimately, like, we'll do our best to dig out all of the…

88
00:10:00.590 --> 00:10:18.399
Joe Worsfold: like, unknown unknowns and everything, but I'm sure there'll be, like, extra features to add, or bugs to fix as we go. I think it's really good that we're having this conversation now, it's nice and early, but, yeah, I'm sure there will be some demons when we come to implement some of it, but should be fine. We've got time.

89
00:10:18.960 --> 00:10:20.189
Kris Mokrzycki: Perfect, thank you.

90
00:10:21.600 --> 00:10:31.180
John Trahearn: Seguing nicely on the demons, because that's probably one of the first things we need to talk about, there is one demon lurking at the moment in InRisk.

91
00:10:31.720 --> 00:10:41.329
John Trahearn: And that is the first story. So, the demon in this case is when you are creating a requirement.

92
00:10:41.880 --> 00:10:47.600
John Trahearn: And you are launch… you launch the party widget to select the client.

93
00:10:48.030 --> 00:10:59.220
John Trahearn: If you can't find the client with Party Search, and then you fall through to the DMB search, and you can't find something there, there is an option to create something manually.

94
00:10:59.670 --> 00:11:05.439
John Trahearn: Now, just the way it's all evolved over the last few years.

95
00:11:05.780 --> 00:11:15.550
John Trahearn: The way originally, within InRisk that you created it manually is that the party widget then deferred back to InRisk to create it.

96
00:11:15.810 --> 00:11:22.480
John Trahearn: That's quite old school now, and in fact, all other areas of, of in-risk.

97
00:11:22.640 --> 00:11:28.489
John Trahearn: It falls to the party widget to create the manual, party.

98
00:11:28.660 --> 00:11:35.609
John Trahearn: But there is one case still remaining which is on the client… on the requirement creation when you're selecting a client.

99
00:11:35.850 --> 00:11:46.889
John Trahearn: So, this first story is about, switching to use the party manual client creation.

100
00:11:47.360 --> 00:11:51.739
John Trahearn: And removing the logic of manual creation on the in-risk side.

101
00:11:52.550 --> 00:11:54.550
John Trahearn: M… I'm gonna…

102
00:11:55.390 --> 00:12:03.540
John Trahearn: open up to, I guess, Billy again, to sort of describe if there's any nuances to us using

103
00:12:04.060 --> 00:12:07.409
John Trahearn: The manual… the manual creation for client.

104
00:12:07.570 --> 00:12:09.490
John Trahearn: On creating a requirement.

105
00:12:12.310 --> 00:12:14.599
Billy Calladine: I don't think so.

106
00:12:16.000 --> 00:12:18.740
Billy Calladine: Can you hear me? I don't know if I'm lagging out.

107
00:12:19.320 --> 00:12:20.030
Kris Mokrzycki: We can hear you.

108
00:12:20.030 --> 00:12:20.530
Billy Calladine: Okay.

109
00:12:20.950 --> 00:12:28.720
Billy Calladine: Cool, yeah, I think that… so, this feature, it already exists, and it's currently being used in the renewals flow.

110
00:12:29.150 --> 00:12:44.270
Billy Calladine: If you get to the point where you want to, renew, but add a new party, you can just do it all contained within the one drawer. So I think there is just a flag that you can enable within, you know, the code that already exists.

111
00:12:44.400 --> 00:12:47.660
Billy Calladine: Say, you know, enable this new… new journey.

112
00:12:47.910 --> 00:12:54.450
Billy Calladine: And, yeah, I think it's working for renewals, it should just work as is.

113
00:12:54.560 --> 00:12:59.129
Billy Calladine: Because the flow that got built is just…

114
00:12:59.390 --> 00:13:03.929
Billy Calladine: replicating the in-risk behavior, where you go to the page, it's the same,

115
00:13:05.120 --> 00:13:08.810
Billy Calladine: Same fields that are rendered on the screen, so… Yeah.

116
00:13:12.090 --> 00:13:22.360
John Trahearn: Yeah, so I believe this one is a fairly straightforward job. It's something that we do already in risk, as Billy mentioned, on the renewals. So there's a…

117
00:13:22.960 --> 00:13:24.640
John Trahearn: So yes, it's just gone into edit mode.

118
00:13:24.790 --> 00:13:29.949
John Trahearn: So there's some configuration so that the manual flow is enabled in

119
00:13:30.060 --> 00:13:35.440
John Trahearn: The widget, and then there's some cleanup to sort of remove all of the manual steps on the in-risk side.

120
00:13:36.890 --> 00:13:38.400
John Trahearn: So that's kind of it for this one.

121
00:13:39.480 --> 00:13:41.770
John Trahearn: Any… questions?

122
00:13:42.050 --> 00:13:48.129
Jason Owen: Does the manual record ever get reconciled? Is that then…

123
00:13:48.350 --> 00:13:52.230
Jason Owen: played back into… I mean, in the New World, it won't be…

124
00:13:53.470 --> 00:14:11.179
Jason Owen: played into the party side, because it'd be created there. But is there any kind of reconciliation of what the… of that manual record, and any kind of, what happens behind the scenes to that manual record? Does it just stay as… that's the de facto record, or is there some kind of…

125
00:14:11.190 --> 00:14:30.529
Jason Owen: testing against that record to make sure that the information's correct? Is there some kind of back-end services that goes and says, okay, there's a flag on this manual record, we need to check that that is, that information's good, or do we accept that what the user has entered into the system is good information?

126
00:14:30.950 --> 00:14:45.080
Joe Worsfold: Yeah, it's a good question. I think it's on our side, essentially, as to whether or not this goes into the curation flow and gets double-checked, which I assume it does for new parties, or just gets taken verbatim. But I think for you guys, let us worry about that, we'll make sure that it's,

127
00:14:45.690 --> 00:14:51.200
Joe Worsfold: You know, whichever of the two, and it remains, you know.

128
00:14:51.360 --> 00:14:59.619
Joe Worsfold: what's the word I'm looking for? That data, the provenance remains… because I think, ultimately, what we're trying to do is stop any…

129
00:14:59.840 --> 00:15:09.429
Joe Worsfold: creation on your side, so that we always have it, and then you can just rely on us to maintain it, but… I know you've got a snapshot of it, but we're trying to stop ingesting it, because that's where we've got some problems.

130
00:15:09.890 --> 00:15:19.740
Kris Mokrzycki: But in terms of question of reconciliation, that is pretty much all good right now, so the minute we switch over, there is no need for any such activity, right, John?

131
00:15:19.880 --> 00:15:25.830
Joe Worsfold: Yeah, it should follow the same pattern we have today, I guess similar to, like, we have for renewals when you're recreating it.

132
00:15:26.950 --> 00:15:27.580
Kris Mokrzycki: Okay.

133
00:15:27.810 --> 00:15:28.780
Jason Owen: Okay, thank you.

134
00:15:32.450 --> 00:15:38.010
John Trahearn: Any other questions on… the manual… Client Creation.

135
00:15:38.890 --> 00:15:40.080
John Trahearn: Changes…

136
00:15:42.880 --> 00:15:43.830
Kris Mokrzycki: Let's do it.

137
00:15:46.050 --> 00:15:46.800
John Trahearn: Yeah.

138
00:15:46.800 --> 00:15:52.069
Joe Worsfold: Yeah, I assume that's the only place, is it? There's… you'll be… I guess you'll be able to find any links to that single page.

139
00:15:52.070 --> 00:15:55.850
John Trahearn: Yeah, yeah, that's the only remaining spot. Yeah, that's confirmed.

140
00:15:57.070 --> 00:16:01.180
John Trahearn: So we're happy to take that to low level, basically, which is good.

141
00:16:02.180 --> 00:16:03.749
John Trahearn: So I'll mark that as such.

142
00:16:05.200 --> 00:16:10.070
John Trahearn: Okay, so that's step one, because, as long as…

143
00:16:10.660 --> 00:16:19.400
John Trahearn: that manual creation remains, it sort of stops us sort of moving towards the new MDM solution, so that kind of clears the decks and allows a clean move.

144
00:16:19.890 --> 00:16:32.669
John Trahearn: So, the next thing is we need to set up our data model to support the new party MDM ID and version ID on our client and broker tables. So that's this second story here.

145
00:16:33.270 --> 00:16:40.900
John Trahearn: Mmm… Yeah, so that's this. So… I mean, it's…

146
00:16:41.100 --> 00:16:50.159
John Trahearn: as it sounds, really, it's a fairly straightforward change on our part. This story is not to do with anything around integrating with a new widget.

147
00:16:50.610 --> 00:16:54.810
John Trahearn: It's purely introducing new… those two new columns.

148
00:16:55.150 --> 00:16:57.690
John Trahearn: So this is really a…

149
00:16:58.120 --> 00:17:03.279
John Trahearn: Data migration on our database, for two tables.

150
00:17:03.510 --> 00:17:09.779
John Trahearn: Adding those two new things to, the relevant tables. Actually, there's Party Snapshot in there as well.

151
00:17:09.990 --> 00:17:18.209
John Trahearn: And then updating all the relevant DTOs, and kind of back-end objects that kind of interact with that, model.

152
00:17:22.940 --> 00:17:25.050
John Trahearn: That's sort of it with this one, really.

153
00:17:25.050 --> 00:17:30.879
Joe Worsfold: Yeah, but if it helps, it's basically, it's UUIDV7 for the party ID, and then just an integer.

154
00:17:31.060 --> 00:17:31.860
Joe Worsfold: For the…

155
00:17:31.860 --> 00:17:32.470
John Trahearn: Back over.

156
00:17:32.470 --> 00:17:33.429
Joe Worsfold: the version number.

157
00:17:35.280 --> 00:17:36.390
John Trahearn: Instrument down.

158
00:17:54.550 --> 00:17:56.390
John Trahearn: Any other questions on that one?

159
00:17:57.290 --> 00:17:58.699
John Trahearn: How can we take that to low level?

160
00:17:59.780 --> 00:18:07.700
Joe Worsfold: I assume, so this will go on both the client table for where you use party as clients, and it'll go on the broker table where you use, parties as brokers, right?

161
00:18:07.920 --> 00:18:15.410
John Trahearn: So, it's actually the party table, although it's a bit confusing, because the party table, the ID of it is client ID.

162
00:18:15.910 --> 00:18:17.749
John Trahearn: It's kind of badly named.

163
00:18:19.660 --> 00:18:25.350
John Trahearn: Yeah, so you'd have it on the party table, the broker table, and we'd also store it into the party snapshot.

164
00:18:26.150 --> 00:18:26.720
Joe Worsfold: Nice.

165
00:18:27.300 --> 00:18:28.550
John Trahearn: They're all independent.

166
00:18:34.680 --> 00:18:35.430
John Trahearn: Cool.

167
00:18:43.620 --> 00:18:49.880
John Trahearn: Okay, and then… so the other three tickets are to do with integrating with the widget.

168
00:18:50.210 --> 00:18:53.770
John Trahearn: And I've separated them out into…

169
00:18:55.050 --> 00:19:02.550
John Trahearn: the sort of logical areas, and as Billy mentioned, actually, it might be that we want to include another one for…

170
00:19:04.750 --> 00:19:11.049
John Trahearn: renewals, although that's, I guess, just confirming the client, so it might be… can swallow that up into this client story.

171
00:19:11.780 --> 00:19:18.050
John Trahearn: Now, the idea here is we want to be… Mmm…

172
00:19:19.220 --> 00:19:22.439
John Trahearn: In a position where this is all feature-flagged, so…

173
00:19:22.660 --> 00:19:25.609
John Trahearn: We'll introduce a feature flag that…

174
00:19:25.810 --> 00:19:38.959
John Trahearn: basically toggles between using the existing workflow with the existing party widget, or when we turn the feature flag on, it will, launch the new party widget, and

175
00:19:39.450 --> 00:19:43.739
John Trahearn: trigger the additional logic of storing those two IDs that will come back.

176
00:19:44.100 --> 00:19:45.860
John Trahearn: Now…

177
00:19:46.400 --> 00:19:54.539
John Trahearn: what I want to confirm with the party team is, is there any… is there any planned change in how we interact with the widget?

178
00:19:56.660 --> 00:20:07.810
Joe Worsfold: Yeah, so… so I was… I was planning to change it, or at least in the sense of I was just building a widget to be how I… how I want it to be for how I use it. But obviously, as we discussed,

179
00:20:08.460 --> 00:20:17.819
Joe Worsfold: that's… we're gonna have to do something different anyway, because of the design… we've got the design system in Chakra V3, so I don't think… that's just not gonna work. So, I think for us.

180
00:20:18.400 --> 00:20:20.630
Joe Worsfold: Probably for the sake of time.

181
00:20:21.530 --> 00:20:25.249
Joe Worsfold: We can build a widget that's bespoke for you guys that is…

182
00:20:25.390 --> 00:20:33.049
Joe Worsfold: gonna match the same sort of auth flow, but I do need to sort of take this one away, probably, with Billy, and… and figure out…

183
00:20:33.260 --> 00:20:33.990
Joe Worsfold: you know.

184
00:20:33.990 --> 00:20:34.620
John Trahearn: Yeah.

185
00:20:34.620 --> 00:20:44.520
Joe Worsfold: Is there anything we dislike about the current foot? Basically, I just need the session stuff and the RBAC controls to be fairly… well, similar, at least, but fairly bulletproof, but,

186
00:20:45.060 --> 00:20:48.179
Joe Worsfold: Yeah, I think for you… for now, I guess…

187
00:20:48.180 --> 00:20:48.630
John Trahearn: familiar.

188
00:20:48.910 --> 00:20:50.330
Joe Worsfold: do parity.

189
00:20:50.620 --> 00:20:52.410
John Trahearn: Yeah, so from a, sort of.

190
00:20:52.550 --> 00:20:59.349
John Trahearn: planning side for us, I appreciate we're kind of in the early stages of this for these two… three tickets, these remaining three tickets.

191
00:20:59.750 --> 00:21:06.609
John Trahearn: M… Do we need to, kind of, bottom out what the widget is gonna be?

192
00:21:07.050 --> 00:21:11.790
John Trahearn: doing before we can take these to low level, it feels like probably that's the case.

193
00:21:11.790 --> 00:21:21.400
Joe Worsfold: Yeah, I think it almost feels like probably need a spike, because there's going to be a lot of questions in low level that I won't be able to answer, which I think the spike will help answer, which is like that.

194
00:21:21.620 --> 00:21:23.689
Joe Worsfold: How is it,

195
00:21:23.990 --> 00:21:30.490
Joe Worsfold: how is it integrated? What was the data that gets returned? I'll probably have to do some work to make sure that the data I'm returning from my, like, open search

196
00:21:30.800 --> 00:21:39.999
Joe Worsfold: Response is what you need. Sorry, it's kind of like, sits in the… squarely in the… in the fence, so we probably need to discuss and… or do a spike with…

197
00:21:40.760 --> 00:21:47.860
Joe Worsfold: a couple of people, or Billy as a… as an ex-striker, he's got more in-risk context as well, but…

198
00:21:48.040 --> 00:21:51.380
Joe Worsfold: Well, sort of between us, I guess.

199
00:21:51.500 --> 00:21:54.409
Joe Worsfold: But I don't know, I think that that was the point where I was like, I guess we…

200
00:21:54.910 --> 00:21:57.110
Joe Worsfold: There's decisions to be made now, which…

201
00:21:57.680 --> 00:22:00.269
Joe Worsfold: It's like, it's an unfortunate thing of…

202
00:22:00.450 --> 00:22:03.249
Joe Worsfold: Do we make it easier for you, and…

203
00:22:03.690 --> 00:22:08.310
Joe Worsfold: extra work for us, which I think we have to anyway, because of the Chakra V3 design system thing.

204
00:22:08.360 --> 00:22:23.629
Joe Worsfold: Because alternatively, I could say, here's my widget, good luck with, using this, it's got, like, TanStack and some other providers and stuff you'll need, and uses an SDK underneath, and you'll end up having to add dependencies, and then dealing with the fact that you'd have to…

205
00:22:23.630 --> 00:22:30.360
Joe Worsfold: tell your package manager that V3 Chakra is got a different name and all this kind of stuff, right? Like, it's… that probably sounds worse than…

206
00:22:30.490 --> 00:22:32.340
Joe Worsfold: Me just making another one for now.

207
00:22:34.780 --> 00:22:40.969
John Trahearn: So let's… let's do that then. Let's put a spike together where we can, kind of, Have a focused…

208
00:22:41.510 --> 00:22:58.459
John Trahearn: session, just… I mean, we've got this sort of… we've got a call later on today, haven't we, Joe, about sort of spinning up the new party stuff locally, which would be kind of the precursor to perhaps having this… this spike session. Yeah. Kind of make sure that what you guys are going to build is going to work, and I guess we're just working on the…

209
00:22:58.660 --> 00:23:00.180
John Trahearn: The premise that…

210
00:23:00.400 --> 00:23:05.839
John Trahearn: For as much as possible, it's going to be a drop-in replacement for now, and then should…

211
00:23:06.140 --> 00:23:08.209
John Trahearn: We need to change things in the future.

212
00:23:08.480 --> 00:23:11.010
John Trahearn: That'll be something that comes separately.

213
00:23:11.180 --> 00:23:11.870
John Trahearn: Is that fair?

214
00:23:11.870 --> 00:23:17.739
Joe Worsfold: Yeah, I think so. I think, ultimately, you guys are always able to push back to us, because…

215
00:23:18.010 --> 00:23:29.059
Joe Worsfold: my original scope was don't affect in-risk too much, so I just went crazy and was like, no, I want it to be really modern and different, but, yeah, we can roll it back a bit until that.

216
00:23:31.300 --> 00:23:39.099
John Trahearn: So, it feels like, for the purpose of the high level today, we've got these stories, we're splitting them into integration

217
00:23:39.330 --> 00:23:48.380
John Trahearn: based on client, broker and party tagging, that's kind of a logical split for our side, but we need to run a spike before we can take these to low level, really.

218
00:23:48.600 --> 00:23:51.150
Joe Worsfold: Yeah, I think you could probably do the, yeah, the…

219
00:23:51.300 --> 00:24:01.699
Joe Worsfold: the other ones, and that is the prerequisite, really, is mostly, like, the data model, I guess, and then, yeah, the wiring is where it's complex, but a spike, I think, will give us way more information for your low-level,

220
00:24:02.240 --> 00:24:04.260
Joe Worsfold: We could just talk to…

221
00:24:04.600 --> 00:24:08.560
Joe Worsfold: I don't know, MR Andrew, and Alex, and figure out whether or not we want to, like…

222
00:24:09.150 --> 00:24:16.690
Joe Worsfold: Line up our sprints so that we've got someone on our side who's free to pair on a bike with some free-risk.

223
00:24:16.990 --> 00:24:24.720
Joe Worsfold: I fear, obviously, only one side having all the… all the information, and then, sort of, if we're throwing stuff over the fence, it'll slow down the…

224
00:24:24.890 --> 00:24:26.880
Joe Worsfold: But you're happy with whatever the.

225
00:24:26.880 --> 00:24:30.579
John Trahearn: How soon can you guys… Dude said Spike.

226
00:24:31.920 --> 00:24:32.720
Joe Worsfold: So…

227
00:24:32.720 --> 00:24:34.029
John Trahearn: Can that be next spring?

228
00:24:36.150 --> 00:24:37.479
John Trahearn: Or is it a bit soon?

229
00:24:37.480 --> 00:24:42.469
Joe Worsfold: it might… that might be too soon, mainly just because I… obviously, I have something, but I only have it in the…

230
00:24:42.810 --> 00:24:48.280
Joe Worsfold: the D3 chakra world with the design studio, so if I'm having to sort of, like, rebuild some of that…

231
00:24:48.730 --> 00:24:57.040
Joe Worsfold: that will take longer, I suspect. Although, you know, anything's possible with cursor these days, I guess, but let me discuss it with Billy and Alex, and then…

232
00:24:57.190 --> 00:25:02.180
Joe Worsfold: We've got an ad hoc high level tomorrow, so I can talk about this one in there.

233
00:25:02.180 --> 00:25:02.680
John Trahearn: Okay.

234
00:25:02.680 --> 00:25:03.580
Joe Worsfold: Back to you.

235
00:25:03.930 --> 00:25:22.249
John Trahearn: So, I mean, I guess from our side, we've got two stories which we can take to low level straight away and make a good start on. That's good news, because we kind of want to get started on this work. We'll wait for you on the widget integration stuff before we can progress these to kind of a more

236
00:25:22.700 --> 00:25:25.179
John Trahearn: Solid, low-level conversation.

237
00:25:25.180 --> 00:25:26.590
Joe Worsfold: Thank you.

238
00:25:27.680 --> 00:25:33.850
John Trahearn: Okay, I think that's kind of it for now, then. I don't know if there's anything else that you guys want to raise, or anyone else has got questions on.

239
00:25:34.720 --> 00:25:40.389
Joe Worsfold: No, I think that's it. I think there's more conversations still to be had for us outside of these, which is mainly around, like.

240
00:25:42.050 --> 00:25:49.779
Joe Worsfold: Sanctions, and also about, like, we'll add those, columns to the data model, and we just decide whether or not we're gonna backfill them with

241
00:25:50.030 --> 00:25:56.469
Joe Worsfold: party IDs that we have for old records, or leave them as null, and go from a point in time, but…

242
00:25:56.730 --> 00:25:59.830
Joe Worsfold: I think we have to discuss that, because it's going to have an impact on…

243
00:26:00.030 --> 00:26:02.380
Joe Worsfold: Mostly on sanctions, but some other stuff.

244
00:26:02.730 --> 00:26:07.700
Joe Worsfold: But yeah, I think all good. Thanks for giving us the time to… To come in and…

245
00:26:08.670 --> 00:26:10.550
Joe Worsfold: Take over some of your high level.

246
00:26:12.380 --> 00:26:13.310
John Trahearn: Always a pleasure.

247
00:26:14.520 --> 00:26:15.860
Joe Worsfold: Awesome. All right.

248
00:26:15.890 --> 00:26:16.360
John Trahearn: Thank you.

249
00:26:16.360 --> 00:26:17.020
Joe Worsfold: Very much.

250
00:26:17.420 --> 00:26:23.210
John Trahearn: Don't know who the… compare is for today's session, but I think that sort of concludes…

251
00:26:23.880 --> 00:26:26.350
John Trahearn: The party MDM stuff for now.

252
00:26:28.600 --> 00:26:29.179
Billy Calladine: Thank you.

253
00:26:29.820 --> 00:26:30.680
Joe Worsfold: Yeah, cheers.

254
00:26:31.550 --> 00:26:32.080
Joe Worsfold: We'll drop.

255
00:26:32.080 --> 00:26:33.540
John Trahearn: Cool. Cheers, guys.

256
00:26:36.530 --> 00:26:46.039
John Trahearn: Andrew? What's… What's next on the agenda, if there is anything?

257
00:26:47.430 --> 00:26:48.449
Kris Mokrzycki: You're on mute.

258
00:26:49.120 --> 00:26:51.099
Kris Mokrzycki: are speechless. I'm speechless.

259
00:26:51.240 --> 00:26:58.369
Andrew Turner: So… I don't know, Rasto or Catty, whether you've got anything prepared for today to run through?

260
00:27:00.050 --> 00:27:21.489
Katarina Voskarova: No, unfortunately, nothing from my side. The tickets that we discussed yesterday on the BA Ketchup, where I found this Ajax ticket, the plan was to have a discussion with VACAD before, the high level, basically before the daily. Unfortunately, we had to move it to the afternoon.

261
00:27:21.490 --> 00:27:23.549
Katarina Voskarova: So, no news.

262
00:27:23.550 --> 00:27:32.999
Katarina Voskarova: On this, and therefore, I'm also not able to present them because, yeah, we don't know what's the plan on the Ajic side right now.

263
00:27:33.000 --> 00:27:34.430
Andrew Turner: Yeah, nope, that's fine.

264
00:27:34.640 --> 00:27:35.720
Andrew Turner: Resto.

265
00:27:35.720 --> 00:27:41.490
Rastislav Sepelak: Yeah, nothing from my side, and we had another meeting regarding Syndicate, so…

266
00:27:41.490 --> 00:27:48.649
John Trahearn: I was about to say, yeah, we've got another syndicate call which overlaps this, so that's probably quite a good segue to perhaps cut this call.

267
00:27:48.650 --> 00:27:49.900
Andrew Turner: Yep, nope, that's fine.

268
00:27:49.900 --> 00:27:50.840
John Trahearn: Okay.

269
00:27:51.290 --> 00:27:53.119
Andrew Turner: Oh, thank you very much. Speak later.

270
00:27:53.120 --> 00:27:55.020
John Trahearn: Lovely. Thanks, everyone. Cheers, bye.

271
00:27:55.020 --> 00:27:55.500
Jason Owen: Bye.

272
00:27:55.500 --> 00:27:56.270
Katarina Voskarova: Bye!

273
00:27:58.020 --> 00:27:58.889
Kris Mokrzycki: I'm cute.

