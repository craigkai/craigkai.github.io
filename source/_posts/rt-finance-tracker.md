# Using RT To Track My Finances

I use RT to automatically track my finances, by using the free Plaid API I am able to pull in transactions from my bank account automatically and create tickets in RT.

First I start by loading an RT group that has each of my users who are using my RT to track their finances:


![Finances group](https://images.ceal.dev/uploads/big/ffdacc16928c174e926a91a49762dfde.png)

Then I map the RT fields to the Plaid API fields that I would like to populate on my finance tickets:

![Finance hash](https://images.ceal.dev/uploads/big/e48148176ef0a2bbc8e65069f590d0fe.png)

I do some date calculations that I won't include here for sake of keeping this post short, once I have my date range of transactions I am looking for I can start looping over my users:

![Credentials](https://images.ceal.dev/uploads/medium/9bb98ea0aa91d50246b7a2ea106620ff.png)

In the above code block you can see that I am querying the Plaid transactions API over a date range for my user. Each user record has a custom field "Plaid access token" which is how we only grab that users transactions.

Now its time to get to the heart of this script and create some tickets from the transactions I found:

![Transactions](https://images.ceal.dev/uploads/medium/586802a7e5799d9907c14935aaf42f12@2x.png)

Now that we have some data we can create reports like this!
financial planner screenshot
