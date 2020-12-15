---
title: svelte-kit
date: 2020-12-14 13:14:37
tags:
---

# Getting Started With Svelte-Kit Session Auth

## Creating our new project
`npm init svelte@next hello-world`

{% asset_img 'code-image' init.png Init %}

Go ahead and type `Y`!

Now we have a new directory that looks like the following:

{% asset_img dir.png Dir %}

For now lets create a simple file based "database" to store our session:

`touch sessions.json`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img session_json.png Session %}
  <p class="code-snippet">Some code here</p>
</div>

Now we want to create another `special` directory for Svelte-kit, the `src/setup` directory.

Inside that directory we will have the following two files:

`index.js`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img index_js.png index.js %}
  <p class="code-snippet">Some code here</p>
</div>

`db.js`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img db_js.png db.js %}
  <p class="code-snippet">Some code here</p>
</div>

The `setup/index.js` file runs for each new request made to the web application. Meaning that so long as we have the cookie with the key value `123` we will be able to authenticate our user. This means we have persistent state!
