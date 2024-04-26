The configuration can be done for free, but I recommend getting the $7 a month for more uptime

# CockroachDB setup
This is the database that will hold user credential data (login info). No credit card is needed, and the free tier should be plenty for thousands of users.

1. Create an account at [cockroachlabs.cloud]
2. Fill out the survey, most options don't matter
3. Choose PostgreSQL

![choose PostgreSQL](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/what%20database.png?raw=true)

4. Click through the tutorial, if you want to skip ahead you can click on the Finish on the left. Click Create cluster

![create cluster](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/finish%20tutorial.png?raw=true)

5. Choose the free option (Serverless)

![serverless](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/serverless.png?raw=true)

6. Choose cloud provider (I choose Google Cloud, aws is also fine both work) and region (pick one close to you!)

![cloud provider](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/cloud%20provider.png?raw=true)

7. Choose Start for free and click Finalize

![capacity](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/capacity.png?raw=true)

8. Choose your cluster name (can be anything) and press Create cluster

![create cluster](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20cluster.png?raw=true)

9. Create a SQL user, any username is fine

![create sql user](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20sql%20user.png?raw=true)

10. Copy the password it generates and store that in a safe place for later

11. The connection window should pop up, if not, click on the connect button in the top left

![connect](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/connect.png?raw=true)

12. Database params: In the `Select option/language` select `Parameters only` and store the four fields here (`Username`, `Host`, `Database`, `Port`) for later

![connection params](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/connection%20params.png?raw=true)

13. Cert Path: In the `Select option/language` select `General connection string`. Expand the `Download CA Cert (Required only once)` section and in `Select operating system` select `Linux`

![cert info](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/cert%20info.png?raw=true)

This will look something like

```
curl --create-dirs -o $HOME/.postgresql/root.crt 'https://cockroachlabs.cloud/clusters/aaaaaaaa-bbbb-cccc-dddd-fafafafafafa/cert'
```

Copy the `https://cockroachlabs.cloud/clusters/aaaaaaaa-bbbb-cccc-dddd-fafafafafafa/cert` part for later

# Log in with google setup (optional)

If you want to let users log in with their google accounts (as an alternative to username + email + password):

1. [Create a new project in the google developer console](https://console.cloud.google.com/projectcreate). Any project name is fine, press Create.

(Make sure you are logged into the Google account that will administrate this, you can change that in the top left)

![create google project](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20google%20project.png?raw=true)

2. Make sure your project is selected in the drop down in the top left, then click on the menu in the top left, `APIs & Services` and `OAuth consent screen`

![oauth consent screen](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/oauth%20consent%20screen.png?raw=true)

3. Select `External` and press `Create`

![create external oauth](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20external%20oauth.png?raw=true)

4. Set the app name (can be anything) and support email (can be yours)

![app name and support email](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/app%20name%20and%20support%20email.png?raw=true)

5. 

# Keycloak Setup

