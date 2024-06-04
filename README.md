# bepisvr.github.io
Bepis VR is an open source social VR app

All the hosting stack is open source too

# Docs

BepisVR is federated (though user data is stored locally, federation is only used for user/world discovery, see [Design](https://github.com/bepisvr/bepisvr.github.io/tree/main/thoughts/Design.md)).

You don't need to follow these tutorials to use Bepis VR! Instead, you can connect to a public instance hosted by someone else.

These tutorials are if you want to host your own instance.

# Networking Stack

## Account Managment

[How to setup the server that holds user accounts, using Keycloak on render.com connected to CockroachDB](https://github.com/bepisvr/bepisvr.github.io/tree/main/doc/accounts)

Edit: This isn't needed anymore for the federated instances, see [Design](https://github.com/bepisvr/bepisvr.github.io/tree/main/thoughts/Design.md)

## P2P Signaling Server

[How to setup a coturn server that lets users establish P2P connections (and also acts as a signaling fallback if P2P doesn't work)](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/README.md)
