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

I regularly attend volleyball tournaments hosted around the Buffalo area. These events are always a great deal of fun, and we are fortunate to have venues and individuals willing to organize them.

That being said, creating a balanced "round robin" schedule and keeping teams on schedule often requires significant effort. The difficulties associated with creating these schedules will be discussed below.

In short, the schedules are usually handmade, and teams frequently end up playing consecutive games in a row or sitting idle for too long, resulting in grumbling and general dissatisfaction.

## the format

Typically, tournaments follow this format:

1. Play X games of `pool` play — This is for seeding before going into playoffs.
2. During pool play, teams are expected to ref other teams' matches (this means refs need to be pulled from teams not playing). But it is considered bad practice to play a game, ref and then play again with no 'downtime' between.
3. Teams should ref the same amount of games where possible.
4. Teams should sit in between games logical amounts.

## the challenges

The main issues with creating a schedule by hand are that teams often end up playing multiple games in a row or sitting for too long.

Compounding these difficulties, there are often only a few copies of the schedule available, whether due to last-minute changes or a lack of physical copies. This means that a central schedule is typically taped up for participants to view, leading to teams often being slow to realize when they should be on the court or, even more so, when they should be refereeing.

I knew I wanted to build something fun and educational using my favorite tech stack (SvelteKit and Supabase). This is the problem I chose to tackle, even though I realize that scheduling tournaments is a problem that has already been solved in many ways.

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

When logged in as an admin user, several CRUD options are available, such as managing Settings, Teams, and Matches, with Brackets coming soon.

I aimed to make the UI as modern and responsive as possible. By using Superforms’ `use:enhance`, form submissions no longer trigger a page reload, making updates to Event values feel seamless and fast. However, this introduced a challenge: What happens when the UI loads and mounts components?

For instance, on initial page load, I load TypeScript objects for a specific event ID and use prop drilling to pass the Event (settings), Teams, and Matches class instances to various components. But if one component updates one of these class instances, other components that depend on the same data must also be aware of these changes.

For example, if I’ve generated Matches for pool play and the UI displays them on an already mounted component, what happens if I change a team name on the Teams tab? Naturally, I’d want the Matches tab to reactively update and display the new team name across all relevant matches.

To handle this, I used the following Svelte 5 syntax in our +page.svelte route:

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

So we use the `$state` feature in Svelte 5 (instead of `team.ts` as the file name it is now `teams.svelte.ts`)!

This means that the UI can now be reactive to when the `teams` attribute on a Teams class instance changes! (It does mean we are using a proxy value which can cause issues in other places if you want the values. Can use `$state.snapshot`)

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
