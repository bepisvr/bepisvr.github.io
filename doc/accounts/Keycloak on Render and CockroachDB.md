The configuration can be done for free, but I recommend upgrading your render.io keycloak server to the $7 a month plan for more uptime

# CockroachDB setup
This is the database that will hold user credential data (login info). No credit card is needed, and the free tier should be plenty for thousands of users.

1. Create an account at [https://cockroachlabs.cloud/](https://cockroachlabs.cloud/)
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

# Keycloak Server Setup

Keycloak is open-source software used for managing user accounts. It will connect to our render server (see above).

The Keycloak server will need to be hosted somewhere, render is a cheap option (free option is available, or $7 a month for 24-7 uptime)

1. Register a render account at [https://dashboard.render.com/register](https://dashboard.render.com/register) or login if you already have an account at [https://dashboard.render.com/login](https://dashboard.render.com/login)

2. Click on `New+` `Web Service`

![new web service](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/new%20web%20service.png?raw=true)

3. Select `Build and deploy from a Git repository` and `Next`

![from git repo](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/from%20git%20repo.png?raw=true)


4. Scroll down to the `Public Git repository` field and enter

```
https://github.com/bepisvr/bepisvr-keycloak
```

then press continue

(Optional: Fork the repository and use your github account's version)

![public git repo](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/public%20git%20repo.png?raw=true)

5. Pick some `Name` (this will be part of the url where your service is hosted) and a `Region` close to you. Make sure Runtime is `Docker`, select `Free`

![keycloak settings](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/keycloak%20settings.png?raw=true)

6. We need to add environment variables:

- add a `PORT` and set it to `8443` (this is very important!)
- add a `ADMIN` and set it to `admin` (this is the username for your admin account in keycloak)
- add a `ADMIN_PASSWORD` and set it to a password you generate (this is the admin account's password in keycloak)
- add a `DOMAIN_NAME` and set it to your app name with a `.onrender.com` at the end. For example, my app name is `my-keycloak-service` so I put `my-keycloak-service.onrender.com`

![env variables port admin and domain](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/env%20variables%20port%20admin%20and%20domain.png?raw=true)

7. Add environment variables from CockroachDB (above):

- `DB_USERNAME` is your CockroachDB's username
- `DB_URL` is the url to your CockroachDB's database
- `DB_DATABASE` is which CockroachDB's database to use (defaultdb is fine)
- `DB_PORT` is the CockroachDB's port

![cockroach db labeled env vars](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/labeled%20cockroach%20db%20params.png?raw=true)

It should look something like this:

![cockroach db env params](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/cockroach%20db%20env%20params.png?raw=true)

8. We need a few more, add:

- `DB_SCHEMA` which should just be `public`
- `DB_PASSWORD` which is the password you generated on CockroachDB's website earlier
- `CERT_PATH` which is the path you got from here

![cert info](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/cert%20info.png?raw=true)

Now it should look something like this

![final env args](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/final%20env%20args.png?raw=true)

9. Now press `Create Service`! You should see something like this

![deploying](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/deploying.png?raw=true)

If you did everything right, your KeyCloak instance should be up and running!

You'll probably see lots of warnings like 

```
BaseServer.NioConnection.readPeerAddress(): cookie read by 10.202.101.222:34242 does not match own cookie; terminating connection
```

Don't worry those are normal (if anyone knows how to fix them let me know!)

10. After 5-10 minutes it should finish setting up, and you should get a green `Live`. Now you can access your server by clicking at the link in the top left

![access link](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/access%20link.png?raw=true)

If all is working you should see this

![keycloak not found](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/keycloak%20not%20found.png?raw=true)

Don't worry this is normal! You need to go to /auth/admin to access your dashboard:

![auth admin](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/auth%20admin.png?raw=true)

Use the password from `ADMIN_PASSWORD` above to log in, and if all is working you should see 

![keycloak homepage](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/keycloak%20homepage.png?raw=true)

Now you are ready to [Configure Keycloak](https://bepisvr.github.io/doc/accounts/Configure Keycloak.md)

