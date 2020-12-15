---
title: svelte-kit
date: 2020-12-14 13:14:37
tags:
---

# Getting Started With Svelte-Kit Session Auth

At the time of writing this Svelte-kit is still experimental but we were still able to get some
basic session auth setup by doing the following.

## Creating our new project
```bash
npm init svelte@next hello-world
```

{% asset_img 'code-image' init.png Init %}

Go ahead and type `Y`!

Now we have a new directory that looks like the following:

{% asset_img dir.png Dir %}

For now lets create a simple file based "database" to store our sessions:

`touch sessions.json`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img session_json.png Session %}
  <p class="code-snippet"></p>
</div>

Now we want to create another `special` directory for Svelte-kit, the `src/setup` directory.

Inside that directory we will have the following two files:

`index.js`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img index_js.png index.js %}
  <p class="code-snippet"></p>
</div>

`db.js`

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img db_js.png db.js %}
  <p class="code-snippet"></p>
</div>

The `setup/index.js` file runs for each new request made to the web application.
Meaning that so long as we have the cookie with the key value `my_web_app-123` we will be able
to authenticate our user without them needing to sign in again.

To add the cookie to the users browser we can use the `Set-cookie` header when returning
the login request:

<div class="code-class">
  <button class="code-toggle">Raw</button>
  {% asset_img header.png header %}
  <p class="code-snippet"></p>
</div>
