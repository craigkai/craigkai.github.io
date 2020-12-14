# Getting Started With Svelte-Kit Session Auth

## Creating our new project
`npm init svelte@next hello-world`

![init](https://images.ceal.dev/uploads/medium/d8a0087f432b87c28129ef12fa101366.png)
Go ahead and type `Y`!

Now we have a new directory that looks like the following:
![dir](https://images.ceal.dev/uploads/big/173fb978ca627cbc23dc3dd707af846d.png)
For now lets create a simple file based "database" to store our session:

`touch sessions.json`

![session_json](https://images.ceal.dev/uploads/big/24aae81a76c28d361e4d22c73e7e7d56.png)
Now we want to create another `special` directory for Svelte-kit, the `src/setup` directory.

Inside that directory we will have the following two files:

`index.js`

![index_js](https://images.ceal.dev/uploads/medium/3084cbfde334c4ebdb5c6d340fc31ba2.png)

`db.js`

![db_js](https://images.ceal.dev/uploads/medium/4d46769f61128fe2e647d008c105160d.png)

The `setup/index.js` file runs for each new request made to the web application. Meaning that so long as we have the cookie with the key value `123` we will be able to authenticate our user. This means we have persistent state!
