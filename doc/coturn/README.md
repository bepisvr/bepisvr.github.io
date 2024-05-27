In order to establish connections, most network configurations require a signaling server.

About 90% of the time, this signaling server should only be needed for initial handshaking, and then users can communicate peer-to-peer. (This handshaking uses a protocol known as STUN)

10% of the time, this doesn't work, and all traffic will be forwarded through the server. (This uses a protocol known as TURN)

Deciding which protocol to use is done by a protocol called ICE.

Coturn [https://github.com/coturn/coturn](https://github.com/coturn/coturn) is the recommended implmenetation, it includes support for all of these.

See [Render hosting](https://github.com/bepisvr/bepisvr.github.io/blob/main/doc/coturn/Render%20Hosting.md) for hosting a coturn server using [Render](https://render.com/)
