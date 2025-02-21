---
title: The Decentralized Web – The Wild West Web
date: 2021-11-01T09:00:00+02:00
author: Stefan Imhoff
description: This is the seconds part of a three-part-series on the Decentralized Web. This part will show some promising examples for the decentralized web.
tags:
  - decentralization
  - software
---

The construction of the DWeb (decentralized web) is in full progress for a few years. It’s going on mostly unnoticed by the public and it’s proceeding slowly. One of the most prominent examples of the decentralized web is BitCoin (and other cryptocurrencies like Ethereum). BitCoin is <q>nothing less than freedom money,</q>[^gladstein2021on] [^fridman2021vm] says _Alex Gladstein_, chief strategy officer of the [Human Rights Foundation](https://hrf.org/).

Except for crypto-currencies few parts of the decentralized work are visible to the public. Working on technologies for the decentralized web is hard, mostly unpaid pioneer work, that might (or not) bring huge wealth to the pioneers in the future.

It takes a lot of time and know-how to solve problems for a decentralized web. New technological solutions need to be developed for the three fundamental components of the internet: **Storage**, **Naming**, and **Databases**.

And developers need a long breath. _Steve McKie_ explained in his Medium article the three generations of the DWeb[^mckie2020ar]: Generation 1 was the development of decentralized Browsers (for example [Brave Browser](https://brave.com/)), of [Ethereum](https://ethereum.org/), the [Ethereum Name Service](https://ens.domains/) (ENS), and [IPFS](https://ipfs.io/) (The InterPlanetary File System).

The 2nd generation is currently being developed: It includes technologies like [Handshake](https://handshake.org/), [Filecoin](https://filecoin.io/), [Ethereum 2.0](https://ethereum.org/en/eth2/), or [WebAssembly](https://webassembly.org/). _KcKie_ estimates another 2-3 years are needed to finalize these.

The 3rd generation will bring decentralized governance, name registrars, content storage apps, web hosting, P2P databases, identity management, mesh networking, and social networking. First contenders of this generation are already appearing.

The DWeb has way too many projects, ideas, and technologies to be able to follow all. If you start researching the topic it quickly becomes overwhelming. It’s nearly impossible to follow all the projects that get created, funded (or not), are discontinued, or die. A lot of ideas are tested and some will prevail. These will get more stable over time and easier to use which is important to bring the ideas to the public.

I picked a few decentralized projects I found personally interesting and will explain them in more detail.

## Mastodon

[Mastodon](https://joinmastodon.org/) is an open-source, distributed Microblogging service similar to Twitter. It is not owned by one company but consists of multiple, decentralized instances owned by private individuals, associations, or companies.

Each instance has its own policies and rules. By default, all instances can talk to all other instances, but it’s possible to restrict the communication to other servers or set up filters for specific types of content.

It’s up to the user if they join a progressive-left LGBT+ instance or a right-wing Christian instance. With technical knowledge and a server, you can even set up your own instance and create your own policies.

It’s possible to mark Toots (the equivalent of a Tweet) with content warnings in case of NSFW, spoilers, or other potential upsetting content.

I like the design and friendliness, but also its weirdness.

## Minds

[Minds](https://www.minds.com/) is a blockchain-based social network. You can earn money or cryptocurrency for using Minds and use the earned tokens to boost posts or support other content creators. Minds has a strong focus on free speech and doesn’t delete content unless it violated a law. They use a 3-strikes system for posting harassment and spam or not tagging NSFW content properly.

Minds has a premium membership program for content creators that want to access exclusive content or monetize their income. But even with the basic account, it’s possible to use the earned tokens to boost the own content.

I like the clean design and how nice and big the content is shown. And the edit feature is fantastic to fix a typo later. With a basic account and a few posts, I earned $20 in tokens in 3 years.

Minds has a build-in encrypted [chat](https://chat.minds.com/) that was recently migrated to Matrix.

## Matrix

[Matrix](https://matrix.org/) is an open network for secure, decentralized communication. It’s feature-rich and allows communication between different servers running (similar to Mastodon). Additionally, Matrix allows communication over Bridging with external services like Slack, Microsoft Teams, IRC, XMPP, Telegram, WhatsApp, Facebook, Hangouts, Signal, and many more.

It’s end-to-end encrypted, supports WebRTC VoIP/Video, and has no single point of control or failure due to its decentralized architecture. Matrix has a feature-set similar to commercial apps and it introduced recently _Spaces_, a feature to group rooms and people together (similar to a workspace in Slack).[^chishtie2021ed]

To connect to the Matrix federation you use a [client](https://matrix.org/clients/). You are not forced to use a specific one and can even create your own. The most used client is [Element](https://element.io/), available for Web, Android, iOS, macOS, Windows, and Linux.

Mozilla switched recently from IRC to [Matrix](https://chat.mozilla.org/),[^gruner2019aa] Minds switched their [chat](https://chat.minds.com/) to Matrix. Gitter joined Element,[^le-pape2020xh] Automattic invested nearly $5 Million into New Vector (the company founded by the core Matrix team in 2017),[^hodgson2020aa] [^lomas2020aa] and Element raised $30 Million.[^le-pape2021xi]

More and more governments use Matrix. The French government forked the messenger and created their own messenger [Tchap](https://www.tchap.gouv.fr), the German states of Schleswig-Holstein and Hamburg use Matrix, and the German military introduced the [BwMessenger](https://www.bwi.de/news-blog/news/artikel/open-source-matrix-ist-einheitlicher-messenger-standard-fuer-die-bundeswehr) for communication.[^loynes2020ie]

Matrix provides a service to [create a link](https://matrix.to/#/@kogakure:matrix.org) for any instance to share with people of different instances to connect.

## Beaker Browser (dat)

[Beaker Browser](https://beakerbrowser.com/) is a fun peer-to-peer Web browser. It is based on the Chromium engine and uses the [dat protocol](https://www.datprotocol.com/) (`dat://`). The [dat foundation](https://dat.foundation/) has other interesting Web3 project as [Digital Democracy](https://www.digital-democracy.org/).

It’s easy and fun to create web projects in Beaker Browser and share them with the whole community. The browser is made for developers and has a JavaScript API. Each browser is automatically a node to share content with all other browsers to create a decentralized network.

## Filecoin

[Filecoin](https://filecoin.io/) is one of the important project achievements of the decentralized web because it solves with a new crypto-currency the problem of hosting content and paying for it.

It will democratize the hosting and allow smaller customers to run servers and provide hosting for a decentralized internet. Centralized hosting makes it easy for the provider to dictate prices and remove or block content.

Decentralization helps with the distribution of content because storage providers can repost popular files and grow with the demand and transport it in all regions of the world, where it is requested, making it faster to access.

If somebody wants to store content they can negotiate with storage providers and pick the best option. The provider earns the storage fee over time. To ensure the data is correctly stored, cryptographic proof verifies the data.

A user who wants to request a file finds a provider that stores the content, pays the price, and receives the file.

## LBRY & Odyssee

[LBRY](https://lbry.com/) is an open, free, and fair network for digital content. Most people access it via the video-sharing platform [Odysee](https://odysee.com/) which is similar to YouTube.

But LBRY is more. It is a protocol (`lbry://`) for any type of digital content, for example, videos, music, ebooks, or video games. It has the aim to become the digital library of the future.

The content of the network is distributed across a network of hosts similar to BitTorrent, but with build-in possibilities for monetization, while the metadata lives on a blockchain.

Content creators can set a price or give the content away for free, content consumers can tip content creators or purchase paid content. LBRY uses its own crypto-currency `LBC` (LBRY Credits).

The project is open-source and the company behind the project, LBRY Inc. build it in a way that it can never become a single-point-of-failure, should it turn evil. LBRY is very censorship-resistant. The video platform [Odysee](https://odysee.com/) has a mild content policy and blocks horrific or infringing content from their server, but the [LBRY Desktop](https://lbry.com/get) app can access every file of the network without relying on the servers of LBRY Inc.

As long as one node is distributing the content it’s nearly impossible to get rid of the content. It’s possible to host content anonymously or within a user's namespace. Popular content is hosted by dozens, hundreds, or thousands of computers, depending on its popularity. The possibility for pretty URLs makes content easily shareable.

This is different from other decentralized video platforms as [BitChute](https://www.bitchute.com/), [DTube](https://d.tube/), or [Dlive](https://dlive.tv/), where the website is usually the single, centralized control point to discover content. Should the website turn evil, or disappear all content is gone.

With the recent aggressive deletions of videos by YouTube of scientists talking about COVID-19 or other controversial political topics, more and more content creators started using Odysee. It provides a YouTube Sync option (for YouTube channels with recent and regular content and a minimum of 300 subscribers), has an iOS app, and an LBRY app for Android. It provides RSS feeds for each channel and it’s possible to download any video to the hard drive.

## IPFS

[IPFS](https://ipfs.io/) is one of the decentralized projects that has huge potential and is already very mature. I first discovered it when the [Brave Webbrowser](https://brave.com/) added native support for the IPFS protocol in January, 2021.[^brave2021yc] [^bondy2021ag]

IPFS stands for _InterPlanetary File System_ and is a peer-to-peer hypermedia protocol (`ipfs://`). This name is not a joke but hints into the possibilities of the protocol in the distant future. Currently, communication between Mars and Earth takes around 13 minutes,[^ormston2012dr] and transmitting 30MB of data can take up to 20 hours.[^nasa2012gh] Not the best conditions for a person on Mars to browse the internet. But IPFS and its features could help with this, by storing hashes on nodes on Mars to reduce the access time to commonly requested files dramatically.

Any file in the IPFS network is split into smaller chunks, which are cryptographically hashed and have a unique fingerprint, a CID (content identifier). It is the permanent record for this file as it exists at that point in time.

Other nodes on the network look up the file and store a copy of it, becoming a provider of the content until the cache is cleared.

IPFS allows the pinning of content to keep it forever. Each node decided this way what content it is interested in.

Each new file or change in the content creates a new hash and thus making it resistant to tampering and censorship. All files stored in IPFS are automatically versioned.

Advantages of IPFS compared to the current web are improved performance because the content is loaded from the nearest location. The bandwidth is improved with data savings improving up to 60% for video. The network is extremely resistant to censorship. The decentralization helps to be resistant in cases of bad connectivity (flaky wi-fi, natural disasters, or in the developing world) and prevents central authorities from mandating rules or limitations on its users.

To run your own node, you can either use [Brave Browser](https://brave.com/), [IPFS Desktop](https://github.com/ipfs/ipfs-desktop), the [IPFS Companion](https://github.com/ipfs/ipfs-companion) extension (available for most browsers) or the [command-line tool](https://docs.ipfs.io/how-to/command-line-quick-start/). If you use Brave and visit the first-time an IPFS address, you’ll be asked if you want to run your own node or use a public HTTP gateway.

The [IPNS](https://docs.ipfs.io/concepts/ipns/) decentralized naming system can help to find the latest version of a file and [DNSLink](https://docs.ipfs.io/concepts/dnslink/) allows mapping CIDs to human-readable DNS names.

---

This is the second part of a three-part series on the decentralized web. In the last part we’ll code and release our first decentralized website.

1. [Why Do We Need It?](/the-decentralized-web-1-why-do-we-need-it/)
2. **The Wild West Web**
3. [Develop and Publish a Website](/the-decentralized-web-3-develop-and-publish-a-website/)

[^gladstein2021on]: Alex Gladstein (2021): _Bitcoin Is Protecting Human Rights Around the World_, <https://reason.com/video/2021/02/05/bitcoin-is-protecting-human-rights-around-the-world/>.
[^fridman2021vm]: Lex Fridman and Alex Gladstein (2021): _#231 – Alex Gladstein: Bitcoin, Authoritarianism, and Human Rights_, <https://lexfridman.com/alex-gladstein/>.
[^mckie2020ar]: Steven McKie (2020): _The Decentralized Web -- Explaining the Impending DWeb Explosion_, <https://
[^gruner2019aa]: Sebastian Grüner (2019): _Mozilla wechselt von IRC auf Matrix und Riot_, <https://www.golem.de/news/chat-mozilla-wechselt-von-irc-auf-matrix-und-riot-1912-145664.html>.
[^hodgson2020aa]: Matthew Hodgson (2020): _Welcoming Automattic to Matrix!_, <https://matrix.org/blog/2020/05/21/welcoming-automattic-to-matrix>.
[^lomas2020aa]: Natasha Lomas (2020): _Automattic pumps \$4.6M into New Vector to help grow Matrix, an open, decentralized comms ecosystem_, <https://techcrunch.com/2020/05/21/automattic-pumps-4-6m-into-new-vector-to-help-grow-matrix-an-open-decentralized-comms-ecosystem/>.
[^loynes2020ie]: Steve Loynes (2020): _BwMessenger goes live for Bundeswehr!_, <https://element.io/blog/bwmessenger-goes-live-for-bundeswehr/>.
[^le-pape2020xh]: Amandine Le Pape (2020): _Gitter is joining Element_, <https://element.io/blog/gitter-is-joining-element/>.
[^le-pape2021xi]: Amandine Le Pape (2021): _Element raises $30M as Matrix explodes!_, <https://element.io/blog/element-raises-30m-as-matrix-explodes/>.
[^chishtie2021ed]: Nad Chishtie (2021): _Spaces: The next frontier_, <https://element.io/blog/spaces-the-next-frontier/>.
[^brave2021yc]: Brave (2021): _Brave Integrates IPFS_, <https://brave.com/brave-integrates-ipfs/>.
[^bondy2021ag]: Brian Bondy (2021): _IPFS Support in Brave_, <https://brave.com/ipfs-support/>.
[^ormston2012dr]: Thomas Ormston (2012): _Time Delay Between Mars and Earth_, <https://blogs.esa.int/mex/2012/08/05/time-delay-between-mars-and-earth/>.
[^nasa2012gh]: NASA (2012): _Communications with Earth_, <https://mars.nasa.gov/msl/mission/communications/>.
