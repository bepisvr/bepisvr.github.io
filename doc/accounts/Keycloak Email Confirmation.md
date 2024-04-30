
# Enable Keycloak Email Confirmations

Keycloak can't send emails by itself, it needs to be connected to a gmail account that it sends emails as.

To attach Keycloack to a gmail, we need to create an app password.

1. App Passwords are only allowed if you have 2-factor authentication enabled. Go to [https://myaccount.google.com/u/1/signinoptions/twosv](https://myaccount.google.com/u/1/signinoptions/twosv) to check, and enable it if it's not (make sure you're on the correct account).

You can also enable 2-step verification by clicking on your picture in the top right -> Manage your Google Account -> Security -> How you sign in to Google -> 2-Step Verification

2. Go to [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords). Select your account with the dropdown on the left and login.

3. Select an `App name` and press `Create`

![create app password](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/create%20app%20password.png?raw=true)

4. Copy the app password it gives and store it somewhere safe

![app password generated](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/app%20password%20generated.png?raw=true)

5. Go to your keycloak dashboard (/auth/admin) select the `users` realm, click on `Realm Settings` and `Email`

![realm settings email](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/realm%20settings%20email.png?raw=true)

6. Set the `From` to your email, and `From display name` to an optional display name

![set from email realm](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/set%20from%20email%20realm.png?raw=true)

7. In Connection and Authentication, put

- `Host` to `smtp.gmail.com`
- `Port` to `465`
- `Encryption` to `Enable SSL` and `Enable StartTLS`
- `Authentication` Enabled
- `Username` your gmail username
- `Password` the app password generated above

![smtp connection](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/smtp%20connection.png?raw=true)

Press `Save`

8. A yellow box will pop up saying

```
To test the connection you must first configure an e-mail address for the current user (admin).
```

Control click it to open that up in a new tab, set the email to your email, and click save (you may need to delete a test user you made if it uses the same email: Users->click on the three dots->Delete)

![keycloak set admin settings](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/keycloak%20set%20admin%20settings.png?raw=true)

9. Now go back to `Realm Settings` `Email`, scroll down, and press `Test Connection`. You should see a message like this

![successful test email](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/accounts/media/successful%20test%20email.png?raw=true)