---
title: The Decentralized Web – Develop and Publish a Website
date: 2021-11-08T09:00:00+01:00
author: Stefan Imhoff
description: This is the third part of a three-part-series on the Decentralized Web. In this part we’ll code and release our first decentralized website.
tags:
  - decentralization
  - code
---

In the last part, we will get our hands dirty and play with IPFS to publish a website. If you want to dig deeper into IPFS later, please have a look at the [official documentation hosted on IPFS](http://docs.ipfs.io).

You can use IPFS Desktop and the IPFS Daemon with the same data, but not run them at the same time. You need to stop the one to use the other. But you can use the IPFS commands to interact with a running IPFS Desktop service.

## Installation

To install [IPFS Desktop](https://github.com/ipfs/ipfs-desktop) you download the binary for your operating system (Mac, Windows, or Linux/FreeBSD).

If you want to use Brave, you can navigate to the browser settings and activate in the **IPFS** section the **IPFS Companion**. You can click on the **My Node** button to open the Web UI. You can change the IPFS Node type in the settings of the companion to use the external node from the IPFS Desktop installation. I haven’t figured out yet if it’s possible to use the command-line tool to access the native Brave IPFS node.

To follow along with the tutorial we use the command-line tool. You can install IPFS via [Homebrew](https://brew.sh/) on a Mac:

```bash
$ brew install ipfs
```

For other options, like the M1 install please look at the [installation instructions](https://docs.ipfs.io/how-to/command-line-quick-start).

## IPFS Web UI

If you installed IPFS Desktop you’ll see the Web UI inside a window which can be accessed through the app itself. You can also open it through the _My Node_ button of the companion extension or by using the URL shown to you when you start the command-line daemon.

The interface has navigation with multiple items: Status, Files, Explore, Peers, and Settings.

**Status** is a monitor of incoming and outgoing traffic of your IPFS node. **Files** show all your hosted and pinned files and folders. **Explore** is an advanced tool to explore hashes. **Peers** shows a world map with nearby peers. **Settings** allows configuring language, public gateway, API address, and other things. The **CLI-Tutor-Mode** is a useful thing for beginners. It shows next to each command in the graphical interface the accompanied terminal command.

## Initializing the Repository

If you didn’t install the IPFS Desktop, you’ll need to initialize the IPFS repository before the first start:

```bash
$ ipfs init
```

## Exploring Files

You can explore objects in your repository with the `ipfs cat` command, for example:

```bash
$ ipfs cat /ipfs/QmQPeNsJPyVWPFDVHb77w8G42Fvo15z4bG2X8D2GhfbSXc/readme
```

Or contents of folders with the `ipfs ls` command:

```bash
$ ipfs ls /ipfs/QmQPeNsJPyVWPFDVHb77w8G42Fvo15z4bG2X8D2GhfbSXc/
```

## Start the Node

To run the node on the command line you have to start the daemon:

```bash
$ ipfs daemon
```

If the IPFS Desktop app is running you’ll get an error that tells you, another process is already using the repository:

```bash
Error: lock /Users/username/.ipfs/repo.lock: someone else has the lock
```

## Add your First File

Create your first file and add it to the repository:

```bash
$ echo "Hello World" > hello-world.txt
$ ipfs add hello-world.txt
```

With the same `ipfs cat` command from above, but using the hash of the `hello-world.txt` file you can see its contents.

## Create a Simple Webpage

We will create now a simple webpage:

```bash
$ cd ~
$ mkdir simple-webpage
$ cd simple-webpage
```

We download an image of a cute cat from IPFS:

```bash
$ ipfs cat QmW2WQi7j6c7UgJTarActp7tDNikE4B2qXtFCfLPdsgaTQ/cat.jpg > cat.jpg
```

Create a new file named `index.html` inside the folder and paste this basic HTML inside:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Nice Kitty</title>
  </head>
  <body>
    <center>
      <h1>Nice Kitty</h1>
      <img src="cat.jpg" />
    </center>
  </body>
</html>
```

We’ll now add this folder and all its content (`-r` for recursively) to IPFS:

```bash
$ cd ..
$ ipfs add -r simple-webpage/
```

You’ll see an output like this, but with different hashes:

```bash
added Qmd286K6pohQcTKYqnS1YhWrCiS4gz7Xi34sdwMe9USZ7u simple-webpage/cat.jpg
added QmY3ayxXcXMs7qdDvPy7QKcVQ492JBNyC9Zf3jp1LrQRCA simple-webpage/index.html
added QmNa1YdawLNeD2fumihKGxuna4GqAws2QsvRsMjzpY2mM2 simple-webpage
 433.02 KiB / 433.02 KiB [==========================================] 100.00%
```

You can see your website now by opening the following URL in Brave:

[https://ipfs.io/ipfs/\<website-hash\>](https://ipfs.io/ipfs/website-hash)

It should also work in other browsers because this URL is using a gateway, but it might be very slow or take a while till the file is available.

In Brave, you’ll see a button in your URL bar to open it directly in IPFS (without a gateway). This will also change the bar to show a IPFS icon and the real IPFS address.

## Publishing to IPNS

Each time you change something with your website, you’ll get new hashes for the changed files and folders affected which makes it hard to always serve your recent version to the world. This is where IPNS (the InterPlanetary Name System) comes into play. You create an IPNS hash that is tied to your Peer ID.

We publish now our website to IPNS:

```bash
$ ipfs name publish <website-hash>
```

You’ll get an output like this:

```bash
Published to <your-peer-id>: /ipfs/<website-hash>
```

This peer id is now available by accessing it in your browser:

[https://ipfs.io/ipns/\<your-peer-id\>](https://ipfs.io/ipns/your-peer-id)

In Brave you can directly access it with the `ipns://` protocol (it might take a few seconds, till it’s available):

[ipfs://\<your-peer-id\>](ipfs://your-peer-id)

You can check where the IPNS is pointing to by using this command:

```bash
$ ipfs name resolve <your-peer-id>
```

If you change your website, add it again to the IPFS repository, and publish it again, you’ll see the updated content at that address.

## Use a New IPNS Name Key Pair

To publish multiple projects at fixed URLs, you’ll need to generate a new key pair:

```bash
$ ipfs key gen <some-name>
```

You can see your keys with this command:

```bash
$ ipfs key list
self
<some-name>
```

You can publish to a different IPNS name by adding another key pair to the publish command:

```bash
$ ipfs name publish --key=<some-name> <website-hash>
```

## MFS – Mutable File System

If you open the IPFS Web UI and navigate to the **Files** section you might wonder, why it’s empty.

The reason is that files in IPFS are content-addressed and immutable. You can’t overwrite them, only create a new version. The hashes in your repository get additionally cleaned up automatically (unless they are pinned) when the cache is full or you run the cleaning task (garbage collection) manually:

```bash
$ ipfs repo gc
```

If you add a file with `ipfs add` it will automatically be pinned.

MFS helps to make working with files more comfortable. We can now copy our website to the MFS:

```bash
$ ipfs files cp /ipfs/<website-hash> /simple-website
```

This will create a new folder at the root of the Mutable File System and copy the contents of the website to it. If you reload the **Files** section of the IPFS Web UI you’ll see the folder, see its contents and hashes. You would use MFS for projects you plan to keep track of and have them stay around longer.

You can use a row of UNIX-like commands to create a folder, write files, copy and move files on MFS. You can find all commands starting with `ipfs files` in the [Command-line reference](ipns://docs.ipfs.io/reference/cli/). This command would create a new file inside a folder:

```bash
echo "Hello, World" | ipfs files write --create --parents /my-new-folder/hello-world.txt
```

## Pinning

If you share content on a decentralized, distributed network, it will only be available, if at least one node serves it. If you’re the only node on the network and shut down your computer, the data won’t be accessible.

[Pinning services](http://docs.ipfs.io/concepts/persistence/#pinning-services) solve this problem for you, they allow pinning CIDs to keep them online. I tried [Pinata](https://www.pinata.cloud/) because they offer 1GB storage for free.

## Hosting IPFS Content With a Domain

There are a lot of different options to serve content with a human-readable name. You could [register an ENS name](https://medium.com/coinmonks/how-to-register-an-ens-name-for-your-wallet-address-190767641dae), and [connect it to your latest CID](https://bitsofco.de/setting-up-a-decentralised-website/), buy a domain at [Unstoppable Domains](https://unstoppabledomains.com/), or use a service like [Fleek](https://fleek.co/), to name a few options.

I use Fleek because I just wanted to play around with IPFS and they offer free hosting. I connected the [GitHub repository of my website](https://github.com/kogakure/website-11ty-stefanimhoff.de/) with Fleek and automatically deploy my website on each push to IPFS. I deploy it currently to a subdomain by adding a CNAME record in my DNS settings, but have bought already an Unstoppable Domain that I still need to move as a <abbr title="Non-Fungible Token">NFT</abbr> to my Ethereum wallet on the blockchain.

## Conclusion

I’m happy with how the decentralized internet is progressing and even though it still feels bumpy and like the early days of the internet. A lot of tinkering, trying out new ideas and technologies, but it is this feeling that makes me hopeful that we witness the beginning of something new.

I hope we will be able to bring back the control of the internet to the masses and give control of data to the individual.

We need to stop malicious and greedy big tech oligarchs and authoritarian politicians from infringing on our rights to life, liberty, and the pursuit of happiness.

---

This is the third part of a three-part series on the decentralized web.

1. [Why Do We Need It?](/the-decentralized-web-1-why-do-we-need-it/)
2. [The Wild West Web](/the-decentralized-web-2-the-wild-west-web/)
3. **Develop and Publish a Website**
