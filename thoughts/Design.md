Local (in a directory, can sync via steam cloud/dropbox/etc.)
- Inventory (sharing items copies them, see below for public folders)
- Your uuid
  - should include the millis since epoch (when account created) to minimize collisions
- Primary username, set in user inventory (see below for rules)
- Profile picture, set in user inventory
- Both username and profile picture are synced whenever P2P connection is made
- Contacts (stores uuids)
- Messages (via uuids, requires P2P connections)
- World Hosting (world stored in inventory, requires P2P connections)

Federated:
- P2P Signaling Server (coturn, tho any other STUN+TURN+ICE should work)
    - Steam is alterantive for P2P connection provider
- Discovery:
    - Public World Discovery
    - Lists of users (a user can be registered at multiple instances)
    - Lists of current public/contact/contact+ worlds (worlds inherit registration from the user hosting them, so can be registered at multiple instances)
    - List of other federated instances to share world list/user list

HTTP/FTP/Git/Dropbox/GDrive/IPFS:
- Meant for persistent data
- uuid -> username mapping
    - Use bluesky's system:
       - User owns a username.domain.etc if there's a file on that domain that says you do
    - steam is one such provider of uuid -> username mapping
       - Specifically SteamID (maybe also store steam username?)
- Public Folders
- Cloud Variables

