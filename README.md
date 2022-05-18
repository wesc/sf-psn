# A Sociotechnical Framework for a Pluralistic Social Network

## Meta
This green paper[^greenpaper] is maintained by Wes Chow at https://github.com/wesc/sf-psn. It is a perpetual work in progress which will be snapshotted on occasion for stable points of reference. I hope we can bring the document to a sufficient level of maturity that the community can develop software around its design.

To contribute or constructively argue a point, do one of the following:

1. Fork this document on GitHub, edit, and submit a pull request.
2. Create a public document with the proposed edits and tweet at [@weschow](https://twitter.com/weschow/). A Google Doc works fine, but if you prefer other formats that is ok too so long as it's public and does not require specialized software to read.

The best contributions are critical, propose solutions, or fill in missing pieces of the argument. That said, you may pose solely critical points by adding content to the "Criticisms" section at the bottom of the document. I will work these in as fluidly as I can. That is not to say I'll accept _all_ criticisms though, especially if I feel it's not relevant or in good faith. Ultimately, I remain the author/editor of this document, and you may always fork or write your own response if you find this work unacceptable.

## Proposition
A pluralistic social network (PSN) framework is a path towards scalable, flexible, and resilient public discourse. We take a definition of pluralism from Glen Weyl: a social philosophy that recognizes and fosters the flourishing of and cooperation between a diversity of sociocultural groups/systems.[^rxc]

Non-pluralistic networks tend to suffer from two extremes:

1. They are global and autocratically controlled, leading to mass misunderstandings, unhealthy incentive structures, and adversarial behavior.
2. They are siloed and opaque, leading to little consensus building action between groups.

How might a PSN encode a pluralistic structure? Like the Coasian idea that clear property boundaries facilitate cooperation, we use the social group as a clear boundary at which we can develop flexible interaction rules. We consider both intra and intergroup rules.

Some design goals for a PSN:

1. Support a plurality of norms and moderation mechanisms. Successful groups should be homogenous in their values to the extent that it produces constructive communication. This homogeneity may be along demographic, interest, or some unknown dimension. We do not a priori presume knowledge of coherent social boundaries.
2. Bridge diverse information between groups through mechanisms that encourage good faith.
3. Reveal an intimacy gradient, where content is bound to norms and its unbinding requires care.

Consilience is the process by which a piece of information is generally accepted as a diversity of viewpoints reinforce the idea. In science, consilience happens through peer review and wide experimentation that confirms a theory. In the social sphere, critical debate and free sharing of information leads to the eventual acceptance of propositions as facts.  Implemented successfully, a PSN embraces natural diversity but also creates explicit bridges that help guide the wider network towards global consilience. It should allow easy experimentation according to group norms, and be resistant to individual and majority takeovers.

This document aims to be just specific enough to establish a focal point for further definition and development. We first proceed with a non-technical theoretical basis for the framework. Then, we move to a technical sketch of how the system might be composed so that we can work with a grounded version of plurality. Both these sections are readable as standalone pieces, though they complement each other. Finally, we propose some practical use cases that arise from the framework.

## Social Theory

### Intimacy Gradient
The intimacy gradient is an architectural pattern described by Christopher Alexander in his classic text, _A Pattern Language_. He points out that spaces in a building have different degrees of intimacy, and argues that good design allows people to "give each encounter different shades of meaning." Social and legal norms dictate that an act of fully public speech, a protest say, should happen on the curb. Bringing the protest into the owner's bedroom is a normative violation; moving bedroom conversations to the curb is too.

While Alexander's intimacy gradient focuses on public/private behavior, gradients exist along cultural and intellectual domains as well. Regardless of your religious beliefs, walking into a Catholic Mass and loudly litigating the existence of God is clearly a violation of norms even though the church is a semi-public space. Critically questioning a divinity scholar lecturing at a university is normatively acceptable despite the lecture hall being a similarly semi-public space, because the university itself is constructed as a place of inquiry.

But mixing normatively different space and speech is confusing: what if the divinity scholar were to give a sermon in a university lecture hall? Does the answer depend on whether 0% or 100% of those in attendance were believers?

Current social networking applications mix space and speech. Posts intended for a certain set of norms regularly get yanked into a foreign context. Application timelines tend to be a mix of speech from total strangers and personal friends with no distinction.

The #killallmen controversy from late 2020 is a salient example. The phrase "kill all men" is not a literal cry to kill all men, rather within some social circles it's an expression of frustration at patriarchy and a form of dark humor intended to relieve anger. The phrase is also literally a violation of Twitter's "hateful conduct" policy which states (emphasis mine):

> You may not promote violence against or directly attack or threaten other people on the basis of race, ethnicity, national origin, caste, sexual orientation, **gender**, gender identity, religious affiliation, age, disability, or serious disease.

A defense of the phrase *must* invoke a normative argument, but this particular norm is not one that's shared globally by all Twitter users. A PSN structurally affords different norms.

### Consilience
A nationalistic society is one in which the borders of a homogenous group are rigid. A pluralist one creates roads by which you might travel and trade ideas.

This trade of ideas is important for a few reasons:

1. What we consider common knowledge now often arrived as an aberrant challenge to the status quo. Relatively small activist groups can drive major social change.
2. Ideas become shared facts when accepted by a plurality of groups. We accept that the earth is round because a diversity of groups depend on roundedness: astronomers, astronauts, pilots, merchants, communications companies. Conspiracy theories are unlikely to be true because they require a diverse set of people to collude.
3. Adversarial systems are necessary for wide scale cooperation. A liberal society is built on top of a variety of adversarial systems: lawyers vs. lawyers, scientists vs. reviewers, journalists vs. politicians. A functioning adversarial system creates the foundation of trust that allows distant parties to cooperate. For example, the existence of a trustworthy court system means two strangers can rely on a contract to conduct business. Accountable public officials can legitimately collect taxes, which in turn fund public goods.

A PSN is a balance between establishing boundaries within which we can debate freely without social consequence and the ability to import and export ideas.

## Technical Sketch
This document does not propose an implementation, rather provides a sketch of how such a system might be designed. In most cases the sketch avoids prescribing technologies like "use JSON." Instead, it says "serialization should have this property" or uses JSON as a hypothetical to illustrate functionality.

### Posts
Posts are the unit of content and forwarding. A post should at a basic level contain text along with some limited number of media objects such as images and videos. The PSN should support extending post types via schemas and reference implementations (see below).

Replies are posts which structurally reference other posts. Replies are generally only viewable to members of a group, though because the reply is itself a post, it is eligible to be a forward.

#### Format and Identity
Posts must be describable by a schema and be uniquely identifiable. One mechanism would be to use a stable serialization format that allows the post to be identified by its hash. Another might be for posts to have identifiers guaranteed to be unique by some trusted third party.

Posts should usually be encrypted such that it's accessible only by the group. There are two situations where posts may not be encrypted:

1. The post is intentionally public.
2. The client implementation is secure enough to the satisfaction of the members that the posts will not be leaked.

Everything, regardless of encryption strength, can always be leaked via screen grab or photo, so the goal is not to absolutely prevent leaks. Instead, we should encode enough friction into the system such that leaking content out of a group is a clear violation of the group's norms.

#### Schemas
There are a variety of different types of posts, including some that have not yet been invented. Schemas should be defined through a community driven "improvement proposal" process common in open source communities.

In addition to the schemas, there should be a community maintained reference implementation in Javascript for each type that renders a post's data as an HTML component usable for web development, similar to how Google's AMP project publishes vetted high quality mobile components. This enables both innovation with post types, and provides a quick path to deployment for a variety of clients.

Reflecting the emphasis on group dynamics, posts are required to be accompanied by a group-of-origin proof, but can have optional authorship. Optional authorship allows for socially shielded posts.

#### Forwarding and Replies
A forwarded post is one that moves from one group to another. The forward should contain the post itself as well as information about the identity of the forwarder. See group receivership section below.

### Groups

#### Identity
Because we focus on inter and intra group dynamics, groups should exist as an unambiguous identifiable entity. A post's origin, different from its authorship, should be provably bound to a group.

A possible mechanism would be to assign the group a public/private key pair. Post origin could be proved through a signature. Such a mechanism would need to deal with the generation, storage, and access to that public/private key pair. A straightforward implementation would be to hold the key pair at a centralized service which mediates access.

A decentralized implementation may need to use an M of N system, which would require that M key holding parts of the system remain online and available for any member to post.

Ideally the origin proof should be self certifying for scalability and resiliency reasons. We would like to decrease the reliance on a centralized check or access to the global internet.

Another mechanism would be to create a global registry, perhaps on a blockchain, of unique group identifiers and members. In such a setup, membership could be managed through smart contracts. Origin proofs might be functions of membership, which could be checked against the registry.

Finally, we should support some mechanism for key migration for two reasons: key pairs can be compromised, and key migration could be used to support group membership changes.

#### Membership
Membership administration is left to a group implementation. We expect there to be a variety of methods of determining membership, as well as banning members. A basic system would allow a small set of admins to make membership decisions, but more complicated structures could exist. One example might be that adding members requires two existing members to vouch for the new one, such as what happens in a social club, and removal requires a majority vote.

The PSN should allow for a proof of membership in order to facilitate receivership logic.

Bots could also be members of a group to support a variety of functions, such as enhancing group behavior, facilitating content recommendation, and simply being fun.

#### Receivership
On receipt of a post, a group implementation must make two decisions:

1. Whether to allow the post within the group.
2. How to prioritize the display of the post to a member.

The PSN has little opinion on the latter point, which may be most influenced by the client.

On the first point, the PSN aims to only allow in posts "in good faith." The intergroup protocol requires that a destination group check for a cryptographically signed consent to forward a message from the origin.

A basic goodness check would be to restrict forwarded posts to those forwarded by a member of the origin and receiving groups. In such a scheme, the member's client would first submit a proof of destination group membership to the origin group, which would then sign a consent message and hand it back to the client. The client would then post to the destination group, which would check for the consent.

The PSN should support upgrading the notion of "in good faith." A likely upgrade might be one that takes into account reputation scores, either at the individual level or group level.

Another possible upgrade might be one that takes into account a social distance between the origin and destination groups, where distance might be defined as a Jaccard intersection.

Yet another might use a quantified measure of goodwill which is accumulated through positive behavior and spent through forwarding.

Finally, a post that is marked as public can be forwarded without goodness checks. This enables public read-only groups.

If we're lucky, all practical goodness schemes can be implemented by adding information to the forwarded posts which are then validated by groups on receipt of a forward.

### Enrichments
Enrichments are extra data that are non-content associated with posts and live within a group. They are optionally but generally not carried with a forward.

Examples of enrichment data are: upvotes and downvotes, moderation flags, proof of currency transaction, reactions, and any kind of group specific information.

### Group Implementation & Transport
The PSN should define a set of interfacing functions for clients. Two core verbs would be:

- `PULL` -- returns all posts and enrichments since a given ordering token
- `POST` -- submits a post or enrichment into the group
- `DELETE` -- deletes a post or enrichment from the group
- `CONSENT-REQUEST` -- a member submits necessary information for the group implementation to approve a forward to a destination group, for example proof of membership in the destination group.

The function of the ordering token is to aid in sorting posts, and also to receive updates. A straightforward ordering token would be wall clock time, but implementations may want to opt for alternatives with nicer properties such as strictly increasing sequence numbers with a centralized implementation, or version vectors with a decentralized system.

Group implementations should provide Javascript reference code for verbs. A centralized group implementation may use HTTP as a transport mechanism, be built with a modified ActivityPub implementation, be decentralized using IPFS or Hypercore, or be implemented on a blockchain. In all cases, the transport mechanism should be provided in the reference code.

### Reference Implementations & Upgrades
We should provide community developed reference implementations wherever possible. Improvement proposals should be accompanied with reference code, including but not limited to new post types, cryptographic proof systems, and forward receivership filters. Ideally, this PSN structure is enough to support continuous upgrades. Let's take three scenarios as examples:

#### Scenario 1
1. The PSN ships with a plain text post type along with code that renders its data representation into a DOM component. Client apps A and B both use the same rendering code and thus display the post the same way, perhaps with some variation in styling such as fonts.
2. Community members define a new image post format and publish working code with the proposal. App A, which considers itself bleeding edge, integrates this working code into their next release. App A is now able to render images, while the more conservative App B displays "not implemented" placeholders for those posts.
3. After some period of time, the image post proposal is formally adopted and the code is moved into a community maintained repository. At this point, App B adopts the format and can render images.

#### Scenario 2
This scenario concerns groups, but is similar in flavor.

Community A uses group implementation A, which accepts posts which are being forwarded in by its members. This goodness check of a post forwarded from B to A relies on: a proof of the forwarder's membership in A, membership in B, and that the message originated from B. The group implementation uses community vetted code that performs these checks.

Community B uses a different group implementation which accepts A's standard but adds in addition to that an experimental goodness check that allows members to forward in posts signed by a third party, for example a music record label. The music loving Community B allows this in their implementation so that they can forward in chats from the label's artists without having to share membership. This goodness check is never formally adopted by the broader community but remains functional and popular.

#### Scenario 3
The world has been struck by a pandemic and a trusted public health agency publishes an algorithm that flags a piece of content as potentially containing damaging advice. Community A decides to install this code for its members because they trust the science. Community B opts to steer clear of this labeling because they are free thinkers.

## Group Theory and Implementation
Group implementations have a lot of room for experimentation so long as they don't violate the intergroup protocol. Some ideas:

- The most basic group implementation would closely resemble a subreddit. It would be centralized, have membership controlled by a small group of admins, support text, image, and link posts, and support upvote/downvote enrichments. Using Reddit as evidence, groups of this size may scale to hundreds of thousands or millions of members.
- Another basic group implementation would resemble a private group chat. Membership might be controlled by a single person, and it might support a variety of post types including animated gifs, stickers purchased from a commercial library, and a variety of reactions with in-group meaning.
- In between these extremes are some interesting possibilities. A group might elect to make all posts public. This would model a setup where a small group of experts, say venture capitalists, exchange links and comment for the benefit of the public. An outsider may find it interesting to "listen in" on this group's commentary.
- A group might elect to make some posts public and keep most private. A city council, for example, may want to privately discuss policies, come to an internal consensus, and then make public statements.
- Because receivership logic is executed by the destination group, an implementation could provide a holding pen for posts by outsiders intended for the group. A group admin could sift through these and approve some for forwarding.
- Group implementations can support higher level constraints on incoming forwards, for example using NLP to reject toxic forwards, or requiring payment in currency or reputation.
- A group might require peer approvals before consenting to an outgoing forward, in effect encoding a group reputation protection mechanism.
- A group might track reputation for all other groups based off interaction patterns on forwards by its members. Suppose, for instance, that a particular set of posts originating from group A forward into B, where members generally downvote them. The group implementation may decide to automatically deny future forwards from A. This is one possible mechanism to restrict the flow of misinformation as judged by the members of B.
- Two groups may implement a dialectic[^dialectic] post type: a post forwarded from A to B receives discussion within B, which results in a rebuttal back from B to A. Group A then combines the criticism in a synthesis. A and B mutually assign each other reputation if the rebuttal and synthesis are deemed in good faith by the respective groups. This is just one example of a broader array of consilience types that can be built on top of the intergroup protocol.

## Where to Next?
This document is merely a frame and lacks enough detail for software developers to collaboratively begin work. We should spend some time refining the framework in public, but also running code is welcome and an effective way to demonstrate functionality. If you do have running code, feel free to submit a PR that adds a link to this section. If you want to collaborate in a hacking group on a proof of concept implementation, tweet at [@weschow](https://twitter.com/weschow).

## References
Jonathan Rauch writes in his [Constitution of Knowledge](https://www.nationalaffairs.com/publications/detail/the-constitution-of-knowledge) about coming to shared reality through a process of open debate: 

> First, by organizing millions of minds to tackle billions of problems, the epistemic constitution disseminates knowledge at a staggering rate. Every day, probably before breakfast, it adds more to the canon of knowledge than was accumulated in the 200,000 years of human history prior to Galileo's time. Second, by insisting on validating truths through a decentralized, non-coercive process that forces us to convince each other with evidence and argument, it ends the practice of killing ideas by killing their proponents. What is often called the marketplace of ideas would be more accurately described as a marketplace of _persuasion_, because the only way to establish knowledge is to convince others you are right. Third, by placing reality under the control of no one in particular, it dethrones intellectual authoritarianism and commits liberal society foundationally to intellectual pluralism and freedom of thought.

Glen Weyl [writes on pluralism](https://www.radicalxchange.org/media/blog/why-i-am-a-pluralist/), the "evolution of a set of social groups," their self sovereignty, and their ability to cooperate. He also [points out](https://twitter.com/glenweyl/status/1513262164177403904):

> To use a formal mechanism to overcome interpersonal conflict, you need a formal notion of a person, which is why public goods solutions depend on anti-Sybil mechanisms. To use a formal mechanism to overcome inter group conflict you need a formal notion of a group, which is why Plurality is key to technology for a cooperative world.

Mike Masnick [argues that moderating content increases free speech](https://www.techdirt.com/2022/03/30/why-moderating-content-actually-does-more-to-support-the-principles-of-free-speech/). The intuition is that communities that are constantly under attack for their beliefs tend to self censor. Creating speech boundaries enables safe spaces in which people can speak up, however without boundary crossing speech we'd get a world of disconnected realities.

Moxie Marlinspike [observes](https://www.youtube.com/watch?v=Nj3YFprqAr8) that decentralized protocols are inherently difficult to evolve. We hope to mitigate this issue by constructing the PSN to be modular and upgradeable, and developing an improvement process modeled after successful open source communities. A small set of committers can make decisions and move the improvement process with pace, while the modular design does not prohibit outside development in the two areas likely to see the most experimentation: types of shareable content and group governance.

In [The Architecture of Complexity](https://www2.econ.iastate.edu/tesfatsi/ArchitectureOfComplexity.HSimon1962.pdf), Herbert Simon describes a watchmaker who designs a process by which he can assemble a watch in segments. If a segment fails, it doesn't destroy the entire watch, and the watchmaker can make progress. But if the watchmaker designs a process by which the entire watch must be made at once, his probability of completion is pitifully low. Similarly, the PSN can fail in many small ways but on the whole makes progress. A globally connected social network without walls can fail all at once.

Cory Doctorow argues that [fixing monopolistic networks is not an answer](https://www.eff.org/deeplinks/2019/07/interoperability-fix-internet-not-tech-companies), rather we should be looking at forcing interoperability to encourage alternatives and innovation. Also see Mike Masnick's [Protocols, Not Platforms](https://knightcolumbia.org/content/protocols-not-platforms-a-technological-approach-to-free-speech) which makes a similar argument and is a foundational philosophy of Twitter's [Bluesky project](https://blueskyweb.org/).

Christopher Alexander describes the "intimacy gradient" pattern in his classic architectural text, _A Pattern Language_. A well designed home, according to Alexander, is one in which there exists a physically legible gradient from public to private space. The kind of speech and behavior acceptable on a person's lawn qualitatively changes as you move inwards and towards the most private space, the bedroom. In most social networks, a user's consumption of public _and_ private content comes through a single feed, and thus blurs and confuses the intimacy gradient.

## Criticisms
VivaLaPanda [argues against legibility](https://vivalapanda.medium.com/the-computationally-post-scarcity-world-e414870be7c4), pointing out that borders are a mechanism for dealing with cognitive labor, and that in a world with infinitely cheap computation this isn't necessary. Speculatively, there may exist a better design that doesn't enforce group boundaries, but rather works with a social graph based reputation system. Suppose, for example, that your email spam filter learns its rules using the ham emails of your contacts as input. Every individual would have a spam filter that behaves according to the norms of their social ties.

## Contributors
Wes Chow - primary author and editor (Twitter: [@weschow](https://twitter.com/weschow), GitHub: [wesc](https://github.com/wesc))

## License
This document is licensed under a Creative Commons [Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/legalcode).

[^greenpaper]: From Wikipedia: a tentative ... consultation document of policy proposals for debate and discussion.

[^rxc]: This framework attempts to hew to the principles of pluralism laid out in the RadicalxChange literature but is disconnected (so far!) from RxC's efforts.

[^dialectic]: The Hegelian dialectic model of debate involves a thesis, antithesis, and synthesis in order to come to truth.
