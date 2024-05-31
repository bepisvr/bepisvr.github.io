1. Go to your [Render Dashboard](https://dashboard.render.com/), click New, then Web Service

![Create New Web Service](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/new%20webservice.png?raw=true)

2. Select Build and Deploy from a Git Repository, and press Next

![Deploy from a git repo](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/build%20and%20deploy%20from%20git%20repo.png?raw=true)

3. Scroll down to the public git repository section and enter

```
https://github.com/bepisvr/bepis-vr-coturn
```

(optional: feel free to fork the repository and use your own copy)

Then press Continue

![public git repo bepis vr](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/public%20git%20repo%20bepis%20vr.png?raw=true)

4. Choose a `name` (can be anything) and a `region` close to you.

Leave `branch` as `main` and `Runtime` as `docker`, `Root directory` should be `empty`

![deploy name and settings](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/deploy%20name%20and%20settings.png?raw=true)

5. Set `instance type` to `free` (you can also do paid, but that's probably not needed)

![instance type](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/instance%20type.png?raw=true)

6. Add two `Environment Vars`:

`AUTH_SECRET` should be a password you generate (feel free to press the generate button)

`REALM_NAME` should be your app name with `.onrender.com` appended afterwards

![environment vars](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/environment%20vars.png?raw=true)

7. Press `Create Web Service`


![create web service](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/create%20web%20service.png?raw=true)

8. Wait for a minute or two, and if all went well, you should see a green `live`! (you may need to refresh a few times)

![deploy live](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/deploy%20live.png?raw=true)

You may see these errors in the logs:

![errors not a problem](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/media/errors%20not%20a%20problem.png?raw=true)

Those are fine and don't matter.
