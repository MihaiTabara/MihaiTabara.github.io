---
layout: post
title:  "Shipping official Firefox Flatpaks"
author: Mihai Tabara
date:   2020-04-15 12:56:33 +0000
tags: [release, mozilla]
---

# tl;dr

_As of the 7th of April 2020, Firefox 75.0 shipped, tagging along with it, was our first official [Firefox Flatpak](https://flathub.org/apps/details/org.mozilla.firefox) that we
automatically published to the Flathub store. The process has long been in the making; aside from the technical bumpy ride,
it tells a story of great collaboration between the community and Mozilla._

# Background

Historically, distributing applications in the Linux world has been challenging, given the large number of distros and their ramifications.
Each of the Linux distributions comes with benefits and drawbacks: building from source, packaging the tarball or having a dedicated _app store_, etc.

In this context, here enter flatpaks. They aim to solve this problem in a generic way, one to cover all (n.b. more on the supported distributions [here](https://flatpak.org/setup/))
the Linux distros. The goal is to have the developers build their applications once and then distribute everywhere, due to the sandboxing concept that allows users
to run application software in isolation from the rest of the system. Security wise, the applications packaged as flatpaks need permissions to have access
to network, files, peripherals, etc, permissions that are defined at the build time of the flatpaks and then later controlled by users on their system.

Once built, flatpaks can be hosted and published anywhere, whether that’s a developer’s organization inhouse storage or some cloud based solution.
The _de facto_ standard has long been established as being [Flathub](https://flathub.org/home), which is the official place to store the flatpaks. Think of it like Apple’s _App Store_ or Google’s _Play Store_.
While distributing flatpaks can certainly work without it, it makes it much easier to centralize everything in one place.

As to shipping updates, think of it like a git repository. When building a flatpak, it’s like building a snapshot of the file tree into a bundle.
When that bundle is sent over to Flathub, under the hood, there is logic that understands which pieces have changed and those are being updated. Users therefore are receiving the updates.

# Firefox Flatpak

At Mozilla, we’ve been already packaging and shipping Firefox into [Snaps](https://snapcraft.io/) for a while now, which is the official way of distributing Firefox for Ubuntu.
The request for Flatpak packaging came about two years ago in [bug 1441922](https://bugzilla.mozilla.org/show_bug.cgi?id=1441922). Being able to distribute Firefox with flatpaks means that we can reach out
more Linux users in a measurable way. While Flatpak adoption in the Linux world is independent from Mozilla, the idea is not to replace the tarball with flatpak but to
provide both, in addition to Snaps, going forward. Any user can still install Firefox via our Mozilla official website, but in addition can also install it via the Flathub store, should they choose for.

Since we’re the Release Engineering team at Mozilla, we own the pipeline that ships the releases into the right buckets. We own the logic that packages Firefox into Snaps, so
naturally we were tasked with packaging flatpaks as well. The road to success was not as straightforward as we anticipated initially and it required a larger collaboration with several actors.

# Bumpy road

We have had a couple of attempts before we finally crossed the finish line. While the packaging principle was straightforward, in practice, things quickly went sideways.

Under normal circumstances, packaging a new format, in the Linux world, at a high-level involves the following:
* grab the official Firefox Linux tarball (e.g. [example of 75.0](https://archive.mozilla.org/pub/firefox/releases/75.0/linux-x86_64/en-US/firefox-75.0.tar.bz2))
* grab the corresponding langpacks for the targeted locales
* bake them together into a specific-formatted bundle using the building tools

However, in practice, aforementioned step three can get very tricky, mainly because of the dependencies needed and the permissions required to run them. With flatpaks, we
have hit some permission errors because, under the hood, the building tools needed some privileged access for some of the system calls. For building-type tasks in [Taskcluster](https://docs.taskcluster.net/docs) we
are using _docker-worker_ which quickly hit this (by design) limitation. We explored other venues such as using generic-workers to overcome the permissions issue.
But eventually, community folks have helped us overcome this by building the flatpak in a different way. All of this however stretched over almost 1.5 years.


# Collaboration lesson

For me, the biggest lesson I have (yet again) relearned is that collaboration is the key to success. While working on this project, we had the opportunity to meet and work with
very passionate community members, from various open-source projects, including but not limited to: Endless OS, Redhat, Flathub, Gnome, Flatpak, and Freedesktop.

While as release engineers we’re subject matter experts for our own shipping pipeline, there were many unknowns and questions when it came to building and shipping the actual flatpaks.
Which dependencies are the correct ones to install? How to properly bake locales? What’s the best way to generate the flatpak and in which format? etc. We didn’t know the answers to these questions but community folks were extremely helpful.

Whether we worked together during hackathons in our Mozilla London office or via online collaboration sessions, the success of this project was mainly a result of a great team effort between community and Mozilla.

# Status

As of the 7th of April 2020, Firefox 75.0 shipped, tagging along with it, was our first official [Firefox Flatpak](https://flathub.org/apps/details/org.mozilla.firefox) that we automatically published to the Flathub store.
Going forward, we’re automatically going to publish there with each Firefox major release. Additionally, three times a week we’re also publishing Firefox Beta to the beta channel of Flathub.

For now, we’re going to let things settle and gather feedback. But in the future, we would like to ship Firefox Nightly, Firefox Beta (and potentially Firefox Devedition) as separate apps (with different application IDs) to Flathub.

# Thanks

Thank you again to both internal and external contributors who have helped along the way with this.
