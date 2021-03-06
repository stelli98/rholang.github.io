---
title: RCAST 44 - FAIRNESS, LIVENESS & RCHAIN
slug: rcast44
author: [rchain stuff]
date: 2019-10-09
tags: ["ladl", "fairness", "liveness"]
excerpt: "Greg Meredith is joined by Isaac DeFrain and Christian Williams to discuss the final stages before Mainnet launch."
---

## RCAST 44: FAIRNESS, LIVENESS & RCHAIN

![ladl](./images/rcast44.jpg)

https://soundcloud.com/rchain-cooperative/rcast-44-fairness-liveness-rchain

Greg Meredith is joined by Isaac DeFrain and Christian Williams to discuss the final stages before Mainnet launch.

Here are the papers discussed in this RCast:

- [Backtracking, Interleaving, and Terminating Monad Transformers](http://homes.sice.indiana.edu/ccshan/logicprog/LogicT-icfp2005.pdf)
- [Categorical Models for Concurrency: Independence, Fairness and Dataflow](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.16.3952&rep=rep1&type=pdf)
- [Fair Bisimulation](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.2.8951&rep=rep1&type=pdf)

### TRANSCRIPT

Greg: Christian, you were just mentioning the connection between species and linear type theories, which is very interesting because Mike and I have been working on this proof theory side of the LADL stuff. I realized yesterday, as we were writing it out, that essentially what’s going on with this use of an enabling catalyst is to factor the linear part. It turns out that you can compose this resource constraint for execution together with any rewrite system. As a result, you’re forcing a kind of linearity into this situation.

It’s a kind of composition. Looked at from the other way, you’re factoring the linear stuff into its own sub-component. That’s how we end up getting a linear aspect of the type theories that we’re generating. It hadn’t occurred to me until just yesterday that you can think of it that way. That ends up being a nice way to factor out the argumentation about how and why this stuff works.

I wanted to talk a little bit today, not about the proof theory side, but about fairness as opposed to liveness. If you actually go and do a literature review about fairness, especially over the last 15 years—this is how I spent my last weekend—there has been some development, but fairness is an interesting consideration and is usually treated somewhat differently than the liveness constraints that one can consider.

In particular, I have long been sensitive to fairness constraints, having built a bunch of actor systems. Carl Hewitt used to joke a lot that actors were strictly more powerful than Turing machines because the demand was that the communication layer be fair. Fairness demands like that are strictly more powerful than you can actually compute inside of a Turing system. He used to make those kinds of comments just to poke people in the ribs to wake them up. It was a humorous comment because it would raise a lot of eyebrows.

But in the case of actor systems, fairness is easy to describe. The fairness in an actor system comes down to the following kind of consideration. Where actors compute by sending messages to each other and they’re buffered from each other by a mailbox. The mailbox effectively represents a message queue. In front of this message queue is what’s called a primitive serializer that serializing the non-deterministic order of the messages into some kind of schedule. There are constraints on that schedule that it be fair.

The characterization can be described in terms of a trio of actors. Let’s say you have one actor that’s in the server role and is serving two other clients. They’re sending messages to the server. One client is very verbose and the other client is terse. You can’t have an infinite prefix in the mailbox of the verbose client’s messages. There have to be some of the terse ones sprinkled in some finite sample. Does that make sense?

Isaac: With the verbose and terse clients, you’re saying they have longer or shorter messages or they’re sending more or less messages?

Greg: More. It’s about the volume of the messages compared to the size of the messages.

Christian: How would it be a prefix if you’re constructing them by binary one to another?

Greg: Because the primitive serializer could just always be unfair—always favor the verbose client over the terse client. Whenever it gets a choice of ordering them, it always puts the other one, because they keep coming in: “Oh, here’s another one from the verbose guy. Put that in front of the other. Oh, here’s another one from the verbose guy. Let’s put that one in front.” That’s how it ends up being unfair.

Isaac: You’re saying that regardless of the order that the messages are received?

Greg: In the sense that as long as they keep coming in. Primitive serializer is giving an ordered view to the server actor.

Isaac: That makes sense. Fairness, you’re saying, is just some well-defined notion of alternation between all of the possible clients’ messages that are coming in.

Greg: That’s how actors view it. If you survey the literature, one of the things that’s really interesting is that a lot of the literature wants to see it as a whole system property, which makes it much harder to verify, much harder to enforce in terms of local code. If you look at bisimulations, Thomas Hildebrandt, who did some work on bigraphs, characterizes history-preserving bisimulation, which is decidable for finite-state systems, and hereditary history-preserving bisimulation, both of which give you enough information to make some decisions about fairness or to describe fairness properties. Hereditary history-preserving bisimulation is not decidable even for finite-state systems.

When you start to take this whole-systems view, as opposed to looking at things in a more local fashion, you can quickly lose the ability to make these things decidable. That’s really important.

There’s one other piece of the puzzle. Shri Rajamani took a look at fairness in the context of CTL and CTL*. The perspective is whole systems. What they were looking at was to whack down the total set of models to just the fair ones and then show that whatever was true in CTL or CTL* was true in the fair subset. You could continue to do your reasoning and factor fairness to the side, which is again, that the more evidence of the kind of perspective that I wanted to submit for consideration, which is that the fairness is typically considered separately from other liveness constraints.

In particular, if we just look at deadlock as a sort of typical liveness consideration, you can have unfair systems that avoid deadlock. There’s always some Uber controller that’s parceling out the work and they guarantee. In fact, the reason they’re put in this position of being the Uber controllers because they guarantee no deadlock. It’s live, but it’s not fair.

Isaac: It’s like a veto player in game theory.

Greg: That’s exactly right. On the flip side, you can have things that are fair but not live. Everybody gets a fair shake, but they get stuck. That’s a way to see that fairness is a little different than some of these other liveness considerations.

Christian: I’m still not quite sure I understand fairness. It has something to do with alternation or proportionality of ability to send. But then you said you could formulate it in terms of bisimulation?

Greg: That’s right. Bisimulation can be seen as governing who sees what messages. You can talk about adding fairness constraints in that kind of setting. Does that make sense?

Isaac: You’re saying just making sure enough people are seeing the messages, or I guess I’m not really sure how to connect those two…

Greg: You can always look at the total aggregate system of all of the agents that are computing. Then you can look at all the transitions of that system. Then you can compare that to a system in which the actors are fair. The way you would do that comparison is bisimulation. Every transition in one is matched by a transition in the other. That’s how the two are related.

The thing that we noticed, which is what we do with Pi calculus and Rho calculus, if you acknowledge or respect the distinction between producer and consumer, then you can characterize the kind of liveness constraints that we were looking at before in a different way than you characterize the fairness constraints. In particular, the synchronization constraints that we’ve implemented are on the production side. A validator only can produce messages if it meets certain considerations with respect to having listened to the other actors in the system—the other validators—that will absolutely force a kind of liveness characteristic on the network. In fact, there’s a whole plethora of different liveness characteristics that you can get by imposing weighted grammars on the specification trees of the blocks that are being proposed.

But you can also do it on the consumption side. Not just on the message production side but on the consumption side. There’s an interesting line of research that has long held my attention that was done by Oleg Kiselyov. He and a couple of his longtime collaborators provided a monad transformer, which they call LogicT. Essentially, it gives fair combinations of streams. Think of a stream as effectively a monad. When you want to aggregate streams together, one possibility, like with a list, is you do an append. But the if in the case of a stream where you have an infinite list, if you do an append and you do it in the wrong order—for example, if you append all the odds to the list one, two, three—there’s your infinite prefix gadget.

Christian: So you zipper?

Greg: You can zipper or you can do other kinds of things. What he did was to provide a general-purpose gadget which he calls LogicT, which implements backtracking. If you think about it, you now want to aggregate across a bunch of streams, not just two. Then you want to aggregate according to certain constraints.

Another application of this kind of machinery is the for comprehension in the Rho calculus, where each port that you’re listening to can be viewed as a stream, but you have pattern-matching constraints. You only want to pull an event off if it matches the pattern. Otherwise, you want to skip it, which is akin to peak. If you do this across a bunch of streams, you potentially want to look at all the combinations until you find the one that matches all the pattern constraints. That becomes obviously a backtracking problem in the general case. LogicT provides that.

I’ve suggested to the dev team, that’s the abstract API. You create a view of each of the validators as if they were providing a stream of messages. You can look at the blocks coming in and you can sort them by producer’s signature. That provides a logical cue. Then you aggregate the logical cues using this fare aggregation method.

The lowest-end version of this is, as you said, Christian, zipper, which is round-robin. That’s the cheapest way to do that, but you can get fancier. Oleg provides a fair disjunction in a fair conjunction. Now you have a little language that allows you to do policies: disjoin these streams and conjoin these streams and then disjoin these streams and so on. You can aggregate up a whole tree of these streams using this little language. Does that make sense?

Isaac: This is all an effort so that nobody’s messages take priority over anybody else’s. Basically, enforcement of decentralization.

Greg: Yes, but it’s not that nobody’s messages take priority over anybody else’s. It’s that you have a priority policy language. That’s really what the LogicT monad transformer is providing.

Christian: What’s the difference between disjunction and conjunction?

Greg: We’re talking about a fair disjunction, so it’s a fair union because the streams are like sets. Disjunction is like a fair union. Conjunction is like a fair intersection.

Christian: What does that mean for streams?

Greg: The simplest answer is you can do an interleaving; you can do a round-robin; but you can do more complicated things. The union is an interleaving.

Isaac: The fairness aspect is just coming in because there’s an implicit order there.

Greg: Because there’s an order and because some of them may diverge. Let’s say you have a list that has an Easter egg in it somewhere. If the computation doesn’t have to touch the Easter egg, it won’t blow up. Depending on how you do the interleaving or how you merge these two lists together, whether you’re providing both things or you’re providing something from both thing, that matches a criteria—that’s the intersection part—how you do that from each stream will matter if one of the streams might contain an Easter egg.

Christian: What do you mean by an Easter egg?

Greg: You touch the thing and it goes into a divergent computation. That’s the other aspect of this. It’s not just that the streams may be infinite, but they may contain Easter eggs. They may contain divergent computations. The typical way to deal with not touching diversion computations is you are as lazy as possible. The thing about LogicT is it allows you to give this mix of laziness and eagerness. That’s the thing that’s cool about this.

Isaac: You’re saying the validator would basically be able to play by their own rules and, to some extent, within the framework of this fairness constraint?

Greg: That’s the idea. As with everything, that’s precisely the point. You elucidated this. What I’ve been arguing is the whole thing with the synchronization constraints was to provide a language in which to express constraints so we weren’t pinned down.

I’m suggesting the same on the fairness side. Don’t pin this down to a one-size-fits-all solution. Provide a language—that’s where LogicT fits in. The nice thing is that LogicT has a very crisp, clear characterization in terms of category-theoretic semantics. The interesting thing about LogicT, though, is it’s a monad transformer and not a distributive law.

Isaac: What do you mean by monad transformer? I think I understand heuristically, but I’m not sure what the actual definition is.

Greg: Without going into a lot of the details, the monad transformer is another way to combine monads and ensure that the result is a monad. In general, when you combine monads, the result is not necessarily a monad. If you have a distributive law it is a monad. That’s easy to prove. The monad transformer is yet another way.

What I’ve argued in the past is that the distributive law is far superior in a lot of cases. I’ve tried to use LADL as an example of a place where you really need it because of its equational characterization that makes reasoning a lot easier. In fact, the equations are precisely what get you out of the stickiness of doing the sum of two theories. In the LADL case, you’re trying to get to this semantics, which is the collection of the terms. You have these terms of collection combos in the combination of the theories. You can’t get out unless you have a distributive law.

Christian: Why can’t we use the same language for the synchronization constraints as for this problem?

Greg: This is what I was trying to do at the top of the conversation: observe how the literature factors fairness. Fairness is typically considered separate. The argument is that we can have systems that are live but not fair. We gave some sketches of what those look like. We have systems that are fair but not live. We gave some examples of those.

Christian: Why does that mean that they can’t have the same language?

Greg: The next piece was to talk about that in terms of the implementation view of the system. The liveness constraints are about the production of messages and the fairness constraints are about the consumption of messages. The production side doesn’t have to worry about Easter eggs, typically, or interleaving things, whereas the consumption side has to worry about exactly those things. The language has to have different primitives, essentially. But they are closely related.

This is the whole RChain approach. It’s basically a language-based approach. At the end of the day, I’m a language geek. I fundamentally believe that most interesting computer science problems boil down to constructing a little mini-language in which to express a variety of solutions.

Isaac: Is it possible that the fairness and the liveness languages that we’re talking about are dual in some respect?

Greg: They are dual, at least in the way that we’ve decomposed the system. If you look at the literature, that duality is not explicit. That’s because they get characterized as these whole system properties.

Isaac: You’re saying from the perspective of the actor, they’re dual languages.

Greg: In fact, not just actors, but agents in the Pi calculous, agents in the Rho calculus, all of those computational models that respect the duality between input and output, actually represented explicitly.

Isaac: Right. I guess you need that on the ground level first before you can even talk about duality.

Greg: Exactly. There are other kinds of duality, like the Morgan duals and things like that. The bisimulation stuff is usually over whole synchronization trees rather than looking at it in terms of the input and output.

As far as I can tell, this is a unique perspective. I have yet to find a paper and or anything in the literature that looks at things like this. Yet it’s readily graspable. I don’t think there’s anything here that’s rocket science. We’ve been able to cover most of the concepts in a short period of time.

The thing that’s really beautiful is that there are these ready-made gadgets that we can pull off the shelf. That’s one of the things that I’ve been trying to follow as much as possible. In the case of the Rho calculus, let’s start with the best models, look at where there are gaps, fix those gaps, and then derive a model of computation that we can reduce and practice from there.

The same with LADL. I’m much more interested in semantics that are built out of stuff we know and try to be super conservative. Mike and I spent a long time, for example, rather than doing everything we could to try to avoid dragging in all of TOPOSIS. Can we build the comprehension language that just uses the monadic machinery? But ultimately you discover that there are certain no-go theorems with respect to distributive laws and so you’re forced to have more and more set-like gadgets. That’s what justifies the use of TOPOSIS. The design methodology, the work ethic, is let’s not invent anything if we don’t have to.

Christian: What did we mean by languages being dual?

Greg: What I was talking about is the duality between production and consumption.

Christian: Do you think this could be some kind of actual mathematical duality?

Greg: Effectively, it’s covariant versus contravariant. It’s just iterated covariant because you’re talking about streams, or iterated contravariant.

Isaac: This is something that will be combined with the liveness constraint that’s already there.

Greg: That’s exactly right. We’re actually ticketing work right now to add the simplest version of this gadget. If it were me, I’d put in the LogicT as the API, and then underneath it, rather than do full-on backtracking, I’d do something simple that meets the equational constraints. Given the time constraints, we may incur a little bit of technical debt and just do the simpler thing and then add the API post-facto, after maintenance. It’s a resource constraint problem as opposed to architecturally what’s going on. We know architecturally and theoretically what’s going on and now we’re putting the best implementation that’ll get the job done. With luck we can provide a better factorization after the fact. Again, math wins.

Isaac: Doesn’t it always? The liveness constraint was initially put into place because of the long proposal times, right?

Greg: Just to be clear, Casper’s not live unless you add some kind of synchronization constraint. We knew we were going to have to do that.

Isaac: This is just the manifestation of that.

Greg: Long proposal times show up as a manifestation. If you crank the synchronization constraint all the way up to 11, the proposal times stay stable. We have the data on that.

Isaac: By that you mean 0.99 synchronous constraint.

Greg: Exactly. Synchrony constraint is just one of a whole host of them. There’s a whole language of synchronization constraints. We just pick the one we can implement as a dial and as fast to check. When you crank it all the way up, then that limits the throughput of the network.

Isaac: It essentially forces a round-robin, or something close to a round-robin.

Greg: It’s kind of round-robin-ish.

Christian: What does that mean?

Greg: It’s not necessarily that you go from validator one to validate or two to validate three. That’s round-robin. You could have validator one going again. You could have some stutter in this as long as they have enough stake in their justifications. They’re permutations on the order that’s supported. You just have to visit all of them.

Basically, what you’ll get is any permutation of all N and then another permutation of all N and then another permutation of all N, like that. So it’s not round-robin. Round-robin will be one, two, three, four five, up to N, then again, one, two, three, four, five up to N. You won’t ever see a permutation.

Christian: That seems the most fair for it to be a sequence of permutations: efficiently random.

Greg: This actually goes to cake-cutting algorithms. It’s most fair in the abstract. When you start to cut a cake, someone says, “I don’t like so much icing. Can you give me the corner piece?” Those kinds of practical considerations, they map over into things like, “Well, this guy has more memory. This guy has more bandwidth. This gal has more compute power.” Exactly the same slot is not actually fair if you take in everything. It’s fair in the abstract, but you can do better. In particular, you want to be able to have some wiggle room in the system.

Derek: We had a brief system crash here. Picking up with Greg again…

Greg: Christian was asking about papers and resources. I’ll send you some links, Derek, to add to the notes. The only thing I was saying, for the benefit of listeners, is that post-Oleg’s paper there were a bunch of people implemented in N Queens and others sort of classical backtracking problems this way, then said, “Hey, it doesn’t perform as well as some optimized backtracking solution.”

When I read the paper, my mind immediately went to, “Oh, this is a great way to aggregate streams,” not “this is a great way to implement backtracking.” This shouldn’t be viewed as a way to structure backtracking, or at least I wouldn’t. I was doing it as a way to fairly composed streams. Having provenance and resources are really important for the research side of this.
