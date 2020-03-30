---
layout: layout.njk
title: Svelte Native
date: 2020-03-26T17:15:32.261Z
---

# Svelte Native and {NS}

I'm A NativeScript developer, should I start my next project in SvelteNative
**Yes** If you've been working with Angular, jump ship. It's time. If you've been working with vue, Svelte is a really easy switch. Much of the Svelte syntax will be the same. And Svelte does have real performance benefits compared with the virtual dom.

I'm a Svelte Developer, with no Mobile development experience, but want to get into that field. How about me.

**Yes**, NativeScript is a fairly mature project, and Svelte is great. This would be a smart place to start, and you could focus your efforts in learning the nuances of Mobile development, and not get bogged down with the convoluted design patterns with react and hooks, which solved some problems, but are still inherently flawed (I'm talking about you useEffect() dependancys nightmares!).

### I'm a ReactNatie Developer, should I switch to SvelteNative

**No.** For better or worse, as of now ReactNative is better than NativeScript.
The eco-system is better.

The core components are better in RN. For example with, NativeScript the List (RN's FlatList) has a pretty poor implementaion, lacking some of the most basic features such as pull-to-refresh by default. And I've yet to work on an app that doesn't have a list.

You can get these features via NativeScript UI `tns plugin add nativescript-pro-ui` but then
you have to do a bunch of porting for SvelteNative, bridging those components.

Plus the NativeScript marketplace doesn't have the breadth of packages the RN community
provides, so if you are thinking of making the jump, plan on spending some time writing your
own packages for a number of things you probably have been taking for granted in RN.

That being said, RN's no golden goose, and a well maintained open source package you depend on today could just as easily be gone tomorrow.

Also there's very little documentation about how to adapt the Svelete syntax to NativeScript So if you've got a fair amount of experience with Svelte, it may be easier to port the packages, but if you'll be fairly new to both platforms, the guides can actually be a bit misleading because it's not super clear how to reconsile native script design patterns with Svelte.

The dream, ReactNative's Native bridging with Svelte! Now that would be somethign

### Does Svelte performance really matter on Mobile?

This is the real question, and I'd love to see some performance charts.
With any JS driven mobile experience, you've got to send all your updates across some sort of device bridge.

### Major Con

NativeScripts HMR is bad. It's really unreliable. And gets hung often. There's also not force relaod command like you have in RN. You have to toally rebuild the NativeScript app after your HMR hangs. Plus the stack trace is absolutely useless in NS, it doesn't even tell you remotely where the code is incorrect.
