Rationale
---------

Everyone uses social networks.  We use them mostly for fun and
amusement, less frequently for education and joint action.

Yet, there is a problem.

Problem called "centralization".

You've might have heard about Johnston's Law: "Everything that can be
decentralized, will be decentralized" [1]

There's an issue, however, which is called Levine Counterpoint:
"Anything Disruptive that Can Be Stopped, Will Be Stopped". [2]
That means that anything, that the powers, controling or influencing
the service, would stop anything disruptive.

Examples are countless:

Censorship or monitoring by governments:

* China's Great Firewall [3]
* Internet censorhip during arab springs [4]
* PRISM by NSA [5]
* Internet monitoring by Russia [6]

Even in Ukraine, the freedom of Internet is constantly challenged [7]

Services are censored by their owners.  Heard of FB banning
firearms posts left and right?  If not - google for "fb gun
censorship" [8]

Even when owners don't play a direct role in a censorship, they
frequently assist it by reacting to complaints.  This is frequently
used by organized groups of trolls to ban posts, groups and people
they disagree with.  Examples are pro-ukrainian and pro-belarus
activists banned by russian groups, which became common case since the
Russian-Ukrainian war started in 2014.

Also, every centralized service suffers from censorship by DDOS.
Since the start of the Russian-Ukrainian war, ddos attacks became
everyday for ukrainian news outlets.  If you're in doubt, just recall
frequent DDOS attacks on bitcoin exchanges.

This won't matter if we would use social networks just for posting
cats.  But socian networks are more and more frequently play a
networking layer for important social actions.  Like Orange Revolution
in 2004 would be hardly possible without LiveJournal and SMS, the
Maidan in 2013 was fueled by FB, Twitter and IM's

I'm an engineer.

So, if I want to stay free, I have to engineer a way for social
networks to become completely decentralized and uncensorable.

Solution
--------

Back in 2013 David Johnston et al introduced a concept of
Decentralized Applications, Dapps [9]

In short, the definition of dapp is:

1. The application must be completely open-source, it must operate
   autonomously, and with no entity controlling the majority of its
   tokens. The application may adapt its protocol in response to
   proposed improvements and market feedback but all changes must be
   decided by consensus of its users. 
2. The application's data and records of operation must be
   cryptographically stored in a public, decentralized blockchain in
   order to avoid any central points of failure.
3. The application must use a cryptographic token (bitcoin or a token
   native to its system) which is necessary for access to the
   application and any contribution of value from (miners / farmers)
   should be rewarded in the application’s tokens.
4. The application must generate tokens according to a standard
   crytptographic algorithm acting as a proof of the value nodes are
   contributing to the application.

Decentralized and uncensorable social action network should satisfy
following requrements:

* content must be stored on several nodes in the network, including viewers
* content storage and retrieval should be connection-agnostic
* content storage and retrieval should be fast
* storage and retrieval should be free or extremely cheap
* it should support well both tiny and huge files
* should be open-source and not controlled by any single owner
* should support both content and apps

What are the alternatives?

* [storj](https://storj.io/)
* [MaidSafe](http://maidsafe.net/)
* [pure DHT](https://en.wikipedia.org/wiki/Distributed_hash_table)
* [BitTorrent](https://en.wikipedia.org/wiki/BitTorrent)
* [IPFS](https://ipfs.io/)
* [zeronet](https://zeronet.io/)
 
I've decided to use IPFS as a storage and data transfer layer

* MIT license
* users store their content, possiblity of running storage nodes
* connection agnostic, heavy caching
* free to use (no intrinsic coin/token/cost) yet filecoin is on the roadmap
* handles tiny files well via DHT
* handles huge files well via Bittorrent's BitSwap
* support both client-server and blockchain-style apps [10]

Technical basics
----------------

Our usual interface to the internet is a browser.  Thus, to facilitate
the use of our social action network, we have to interface it with the
web.

To do this, we have to implement 3 basic http primitives [11]

The rough schematics follow.

### HTTP GET

* (browser) http GET
* (proxy) ipfs GET
* object content returned to the browser

### HTTP POST

* (browser) http POST
* (proxy) stores POST in the ipfs
* (proxy) connects to the specified ipfs node and sends the hash of
  the POST
* (app server) performs the action and returns the hash of the
  response 
* (proxy) returns output received from the remove ipfs node

### HTTP WebSockets

* same as POST above, but connection is not closed

### Packaging

Proxy would be available both in source code form (github) and
easy-to-install packages for popular OS and mobile phones.

End-to-end encryption
---------------------

Primitives above allow for clear text communication.  But we need to
have some privacy.

End-to-end encryption would be achiveved by

* keys are in user's storage
* user's storage is secured by anything practical, for example:

 * being a stored in a local client app
 * ripple-style, or airbitzh-style blob-storage
 * hardware-based authentification
 * BlockAuth [12]
 * DashSocial [13]
 * Google/FB/Twitter/OpenID

Applications
------------

Ability to transfer packets of data, even encrypted packets of data
has no value.  Solving real world problems is valuable.

This is the job for Applications.

The proposed architecture supports both client server [14] and
blockchain-style [15] native applications.

User also can access existing webapps, like facebook, using reverse
proxy.

Existing webapp could be converted by app developer just by switching
the app server.  For example, if you're using `uwsgi` like we do, just
switch to its ipfs equivalent `ipfswsgi` [16]

Writing blockchain style apps is easy, since ipfs provides lots of
necessary storage and data transfer primitives and you're free to
choose the implementation language and libraries.

The code could be easilty server directly from IPFS and executed on
remote servers over IPFS.

### App Store (not TM :)

To make apps discoverable we will introduce the application store.

It should include:

* Messaging
* Personal blog (twitter or FB-style)
* Personal feed (twitter, FB or reddit-style)
* Groups (FB-style)
* Group governance
* Finances

Who mentioned soundclount/spotify and youtube/popcorn? :)

Uncensorable Groups, Group governance and Group Finances would
radically change the ability of people to cooperate.

Fundraising and governance
--------------------------

To support the development of marketing of the project we will
announce the fundraising.

There would be created 10 000 000 tokens on Counterparty that would be
released on the following schedule:

* 1st month: 1 000 000 tokens
* each following month: 90% of previous month

50% of monthly token distribution would be reserved as for the project
team, 50% of monthly token distribution would be distributed
proportionally to the donations, collected during the previous month.

Funds would be collected to a 2-of-3 multisig counterparty address:
 - 1 key would be stored in a backup
 - 1 key would be controlled by a blockchain corporate governance
   system [CoVote](http://covote.io)
 - 1 key would be controlled by an elected Treasurer

CoVote allows to conduct motions and votings on the blockchain.  Token
holders would be able to decide on
 - payments from the project multisig address
 - elections of Treasurer and subsequent project funds transfer
 - change of the backup key and subsequent project funds transfer
 - the proportion of tokens, distributed between project team and
   donation addresses

As soon as token holders would decide that project's Group, Group
Governance and Finances are ready for production use, tokens, funds
and decision-making would be transfered there.

So, Who is John Galt?

Literature
----------

1. http://www.johnstonslaw.org/
2. https://letstalkbitcoin.com/blog/post/does-uber-know-its-the-napster-of-the-decentralized-sharing-economy
3. https://en.wikipedia.org/wiki/Great_Firewall
4. https://en.wikipedia.org/wiki/Internet_censorship_in_the_Arab_Spring
5. https://en.wikipedia.org/wiki/PRISM_(surveillance_program)
6. https://en.wikipedia.org/wiki/Great_Firewall
7. https://en.wikipedia.org/wiki/Internet_in_Ukraine#Internet_censorship_and_surveillance
8. http://thefreethoughtproject.com/facebook-announces-censoring-talk-firearms-ammo-gun-parts-trading/
9. The General Theory of Decentralized Applications, Dapps https://github.com/DavidJohnstonCEO/DecentralizedApplications
10. https://github.com/akhavr/publications/blob/master/2015/ipfs-services.md
11. Well, yes, I know that HTTP verbs are different, but one can
    implement 99% of any web service functionality using just these 3
    primitives
12. https://github.com/TechEndeavors/BlockAuth.com/blob/master/Whitepaper.md
13. https://dashtalk.org/threads/evolution-social-wallet-discussion.7233/
14. https://github.com/42cc/evox
15. Sorry, still in closed development phase
16. Still in closed development
