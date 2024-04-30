# Log into Keycloak with google setup

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

5. Set the app homepage to your static site and `Authorized Domains` to your static site and your keycloak service. Add a developer contact information email, then click  `Save and Continue`

![oauth app domain and auth domains](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/oauth%20app%20domain%20and%20auth%20domains.png?raw=true)

6. Click `Add or Remove Scopes` and select `.../auth/userinfo.email` and `openid` then press `Update`

![add oauth scopes](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/add%20oauth%20scopes.png?raw=true)

it should now look like this

![scopes updated](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/scopes%20updated.png?raw=true)

press `Save and Continue`

7. Add some emails to test users that you'll use to test. Click on `Add Users`, enter the emails, then press `Add`

![add test users](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/add%20test%20users.png?raw=true)

![test users added](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/test%20users%20added.png?raw=true)

Now press `Save and Continue`

8. Review that everything looks correct, then press `Back to Dashboard`

![back to dashboard](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/back%20to%20dashboard.png?raw=true)

9. Now we need to create the Oauth client id used in Keycloak. Click `Credentials`, `Create Credentials`, `Oauth client ID`

![create oauth client id](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20oauth%20client%20id.png?raw=true)

10. Set:
- `Application Type` to `Web Application`
- `Name` to whatever you want
- `Authorized Javascript Origins` to the url of your keycloak server (make sure there's no / at the end)
- `Authorized redirect URIs` to the url of your keycloak server

![create oauth client id fields](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20oauth%20client%20id%20fields.png?raw=true)

The press `Create`

11. Store the Client ID and Client secret for later

![oauth client created](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/oauth%20client%20created.png?raw=true)

12. Go back to your Keycloak Dashboard, select the `users` realm, click `Identity Providers`, and select `Google`

![identity provider google keycloak](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/identity%20provider%20google%20keycloak.png?raw=true)

13. Leave the Redirect URI as default, and put in the `Client ID` and `Client Secret` from the previous step, then press `Add`

![add google provider](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/add%20google%20provider.png?raw=true)

14. We need to add that Redirect URI to our Oauth 2.0 Client ID. To do this, Click on `Credentials` in the google developer console and click on the client id we created

![modify client id](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/modify%20client%20id.png?raw=true)

Add that `Redirect URI` to the list

![authorized redirect uris add](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/authorized%20redirect%20uris%20add.png?raw=true)

15. To test if it's working, go to your static site, and click "Login" (register won't use google). You should see something like this

![google login screen](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/google%20login%20screen.png?raw=true)

Login with one of your test google accounts.

16. Now go back to the Keycloak admin dashboard, select the `users` realm, and click on `Users`. If all is working you should see that gmail there

![see google user](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/see%20google%20user.png?raw=true)

17. Optional: If you want to make users that login with google automatically verify their email (since they had to use it to log in), you can do this by going to `identity providers` and clicking on `google`:

![select google identity provider](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/select%20google%20identity%20provider.png?raw=true)

In the settings tab, scroll down until you see `Trust Email`. Enable that

![trust email gmail](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/trust%20email%20gmail.png?raw=true)

You can test that this is working by registering a new user with gmail, going to `users` and clicking on that user, and under `details`, `Email verified` should be set to `Yes`