---
title: volleyhub
date: 2024-08-13 14:00:45
tags: sveltekit, sveltejs, supabase, vercel
toc: true
---

# SvelteKit (SvelteJS 5) + Supabase + Vercel

## Table of Contents
- [SvelteKit (SvelteJS 5) + Supabase + Vercel](#sveltekit-sveltejs-5--supabase--vercel)
  - [Table of Contents](#table-of-contents)
  - [the problem](#the-problem)
  - [the format](#the-format)
  - [the challenges](#the-challenges)
  - [the tech](#the-tech)
    - [vercel](#vercel)
    - [supabase](#supabase)
    - [svelte 5 and sveltekit](#svelte-5-and-sveltekit)
  - [The results](#the-results)
    - [A reactive frontend](#a-reactive-frontend)
    - [realtime updates](#realtime-updates)
    - [demo](#demo)
  - [conclusion](#conclusion)

## the problem

I regularly attend volleyball tournaments hosted around the Buffalo area. These events are always a great deal of fun and we of the Buffalo area are so fortunate to have venues and individuals willing to put them one.

That being said, often at these events, there is a significant amount of effort that goes into creating a balanced "round robin" schedule and keeping teams on this schedule. Some of the difficulty around creating this schedule will be discussed below.

But the short of it is regulary, the schedules are handmade, and often teams end up playing consecutive games in a row or sitting for far too long, resulting in grumbling and general dissatisfaction.

## the format

Typically, tournaments follow this format:

1. Play X games of `pool` play — This is for seeding before going into playoffs.
2. During pool play, teams are expected to ref other teams' matches (this means refs need to be pulled from teams not playing). But it is considered bad practice to play a game, ref and then play again with no 'downtime' between.
3. Teams should ref the same amount of games where possible.
4. Teams should sit in between games logical amounts.

## the challenges

The issues that arise with creating a schedule by hand are that teams often end up playing multiple games in a row or sitting for too long.

Further adding difficulties, is most often there are only a few copies of the schedule. Whether due to last minute changes or physcial copies not having been made. This means that a central schedule is taped up where participants can view it. Resulting in teams often being slow to realize when they should be playing on the court or even more so when they should be ref'ing.

I knew I wanted to build something for fun and learnings purposes using all of my favorite tech ([SvelteKit](https://kit.svelte.dev/) and [Supabase](https://supabase.com/)). So this is the problem that I chose to tackle (I know scheduling tournaments is a solved problem already).

## the tech

### vercel

> "Vercel provides the developer tools and cloud infrastructure to build, scale, and secure a faster, more personalized web."

[Vercel](https://vercel.com/) is an amazing product and a logical choice for deploying my SvelteKit infrastructure. By utilizing the [Vercel adapter](https://kit.svelte.dev/docs/adapter-vercel) for SvelteKit. Getting my application up and running in the cloud only took minutes. By using Vercel, I had easy access to logs, and I find it much more simple than using the AWS console! Deployments for branches and PRs are automatically generated and reviewable.

![Vercel Dashboard](/images/volleyhub/vercel.png)

### supabase

Another product that has truly impressed me!

> "Supabase is an open-source Firebase alternative. Start your project with a Postgres database, Authentication, instant APIs, Edge Functions, Realtime subscriptions, Storage, and Vector embeddings."

[Supabase](https://supabase.com/) has made things such as Auth and real-time functionality a piece of cake! Supabase handles user Auth and email (amongst other Auth/Notification methods) but also makes developing a dream. Typically creating and managing a local test DB can be a pain, and I truly dislike when engineering teams all use one dev cloud test DB (you never know how messed up the DB state is).

Supabase makes it easy to spin up a local Supabase web studio along with a database instance to allow for easy and effective DB testing and version control!

![Supabase](/images/volleyhub/supabase.png)

### svelte 5 and sveltekit

[Svelte](https://svelte.dev/) is amazing, and working with Svelte 5 has felt really good! Swicthing to runes was actually very straightforward and along the way I feel I've learned some better practices as far as reactivity in the frontend goes.

## The results

### A reactive frontend

When logged in as an admin user there are a few CRUD options that they can act on. Settings, Teams, Matches and soon Brackets.

I wanted to make my UI as flashy and `modern` as possible. Superforms `use:enhance` made so that subbmitting forms didn't reload my page so updating Event values is really fast and feels great. But there was an issue with this. What happens when I load my UI and mount my components:

| Settings Tab | Teams Tab | Matches Tab |

And on initial page load I loaded my TypeScript objects for some event Id. I am prop drilling where on page load I load my Event (settings), Teams and Matches class instances but if one component updates one of these class instances, that may require other components to be aware.

For example if I have generated Matches for pool play, and the UI is displaying the matches on an already mounted component. What happens if I change a team name on the Teams tab? I will want the Matches tab showing all the matches to also reactively update the dislayed team name to show the new team.

Since we are prop drilling we have something like this (Svelte 5 syntax) on our +page.svelte route:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img propDrilling.png propDrilling %}
  <p class="code-snippet"></p>
</div>

We need attributes on our `Teams` class to be stateful:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img teams.svelte.ts.png teams.svelte.ts %}
  <p class="code-snippet"></p>
</div>

So we use the `$state` feature in Svelte 5 (instead of `Team.ts` as the file name it is now `Teams.svelte.ts`)!

This means that the UI can now be reactive to when the `teams` attribute on a Teams class instance changes! (It does mean we are using a proxy value which can cause issues in other places if you want the values. Can use `$state.snapshot)

So now when I render a match instead of reloading all my matches that contain the updated team I can do something like this:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img viewMatches.png viewMatches %}
  <p class="code-snippet"></p>
</div>

Where if we update our Team value it will reactively update the matches component UI!

### realtime updates

One of the features I was excited to try out from Supabase was the easy real-time updates that the JS SDK could subscribe to. With a little bit of code:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img subscribeToChanges.png subscribeToChanges %}
  <p class="code-snippet"></p>
</div>

Realtime updates are displayed on the `matches` tab:

![Matches](/images/volleyhub/matches.png)

I could easily have my Svelte components update in real-time when match results were input. This means when an event organizer puts results in, users who have the event open on their mobile devices will get the update pushed straight to their UI without a page refresh.

### demo

{% asset_img demo.gif demo %}

## conclusion

Building a scheduling tool for volleyball tournaments using SvelteKit and Supabase has been a fun and educational experience. It addresses the common issues faced during tournaments and helps in creating a balanced schedule efficiently.
