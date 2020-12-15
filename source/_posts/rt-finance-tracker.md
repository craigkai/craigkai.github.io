---
title: rt-finance-tracker
date: 2020-12-12 13:15:44
tags:
---
# Using RT To Track My Finances

I use RT to automatically track my finances, by using the free Plaid API I am able to pull in transactions from my bank account automatically and create tickets in RT.

First I start by loading an RT group that has each of my users who are using my RT to track their finances:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img finances_group.png Finances Group %}
  <p class="code-snippet"></p>
</div>

Then I map the RT fields to the Plaid API fields that I would like to populate on my finance tickets:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img finance_fields.png Finance Fields %}
  <p class="code-snippet"></p>
</div>

I do some date calculations that I won't include here for sake of keeping this post short, once I have my date range of transactions I am looking for I can start looping over my users:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img finance_loop_1.png Loop %}
  <p class="code-snippet"></p>
</div>

In the above code block you can see that I am querying the Plaid transactions API over a date range for my user. Each user record has a custom field "Plaid access token" which is how we only grab that users transactions.

Now its time to get to the heart of this script and create some tickets from the transactions I found:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img finance_loop_2.png Loop %}
  <p class="code-snippet"></p>
</div>

Now that we have some data we can create reports like this!
financial planner screenshot
