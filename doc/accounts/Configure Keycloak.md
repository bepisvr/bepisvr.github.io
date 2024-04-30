
Before this, you'll need to [setup](https://bepisvr.github.io/doc/accounts/Keycloak on Render and CockroachDB.md) Keycloak on Render, attached to a database.

# Frontend setup

We need to make an accounts webpage that the users will use to log in to their account and interact with our Keycloak server. This can be a static webpage, which we can host for free on github pages or render.com. This guide will use render.com.

## Render.com frontend setup

1. Log in to [https://github.com/](https://github.com/) (or make an account), then navigate to [https://github.com/bepisvr/bepis-vr-frontend](https://github.com/bepisvr/bepis-vr-frontend)

We will need to fork this repo, so click on

Todo: fork and modify settings

2. Go to [https://dashboard.render.com/](https://dashboard.render.com/) and click `New` `Static Site`

![new static site](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/new%20static%20site.png?raw=true)

3. Scroll down to the `Public Git repository` section and enter your repository url. For example, mine is

```
https://github.com/bepisvr/bepis-vr-frontend
```

Then press `Continue`.

![static site from github](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/static%20site%20from%20github.png?raw=true)

4. Choose a `Name`, this will be the url of your website. For example, I chose `bepis-vr` so my website will be `bepis-vr.onrender.com`. Set `Publish Directory` to `.` (that's a period symbol). Leave the other options as default, then click `Create Static Site`

![create static site](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20static%20site.png?raw=true)

5. Wait for the green `Live` symbol to appear, then click on the link in the top left to view your site.

![wait for live static site](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/wait%20for%20live%20static%20site.png?raw=true)

# Keycloak Setup

Now you have a static site, we can configure Keycloak to use that site to login.

1. Go to whatever-you-called-your-keycloak-service.onrender.com/auth/admin and log in using the `ADMIN` and `ADMIN_PASSWORD` credientials you set in the previous steps. If you are using the free tier you may need to wait a few minutes for it to start up

![auth admin](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/auth%20admin.png?raw=true)

Once you login you should see this page

![keycloak homepage](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/keycloak%20homepage.png?raw=true)

2. We need to make a users realm to hold our users. Click on the three bars in the top left, click on the dropdown that says "Keycloak", and click Create realm.

![create realm](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20realm.png?raw=true)

3. For the realm name, type "users" (you can do some other name if you want), and press Create. Wait 1-2 minutes while the changes go through, then refresh the page.

![create users realm](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20users%20realm.png?raw=true)

Once you see the users realm in this dropdown, it may be worth double checking your CockroachDB database is connecting to your KeyCloak server properly. You can do this by going to your render dashboard, clicking Manual Deploy, and "Deploy latest commit". Wait 2-3 minutes for it to start up, then log back into your dashboard. If you still see the users realm (and not just master), your database is connecting properly!

4. Select the users realm

![select users realm](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/select%20users%20realm.png?raw=true)

It should look like this

![welcome to users](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/welcome%20to%20users.png?raw=true)

4. Now we need to create a client in the users realm that'll hold all of our users. Click `Clients` and then the `Create client` button

![create client](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20client.png?raw=true)

5. Enter a `Client id`, `Name`, and `Description`. These are up to you. Then press `Next`

![create client general settings](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20client%20general%20settings.png?raw=true)

6. Enable `Client authentication` and `Authorization`, then press `Next`

![client capability config](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/client%20capability%20config.png?raw=true)

7. In all of these fields, put your url for your static site you made above. Mine is

```
https://bepis-vr.onrender.com
```

but yours will be different. Then press `Save`.

![create client login settings](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20client%20login%20settings.png?raw=true)

8. Wait 10-20 seconds for the changes to go through, and you should be redirected to a page like this:

![successful create client](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/successful%20create%20client.png?raw=true)

If you don't get redirected, you may have been too slow. Because you are using the free version this might hang because your server was spun down, refresh to make it automatically spin up again and try doing it faster this time.

To double check your client exists, you can also go the `Clients` menu (make sure you have the `users` realm selected)

![client exists](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/client%20exists.png?raw=true)

9. By default KeyCloak requires you to manually create each user account. We want users to be able to create their own accounts. To enable this:
- Make sure you are in the `users` realm
- `Realm Settings`
- `Login`
- Enable `User registration`

![need to enable user registration](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/need%20to%20enable%20user%20registration.png?raw=true)

It should redirect you, once it is done loading you should see this

![enabled user registration](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/enabled%20user%20registration.png?raw=true)

10. Let's test our user auth workflow. Go to the static site and click "Register", you should see a page like this

![register screen](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/register%20screen.png?raw=true)

Now enter some values for a test user (it doesn't matter what), and press `Register`.

![user register values](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/user%20register%20values.png?raw=true)

11. Go back to the admin console, make sure the `users` realm is selected, and click on `Users`. If all went well you should see the user in the list

![user populated](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/user%20populated.png?raw=true)

Now KeyCloak is all setup!

Next, there are some optional steps:
- [Log in with google support](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/Keycloak%20Login%20with%20Google.md)
