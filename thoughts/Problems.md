There are a few problems to solve:

- User Account Managment
  - Keycloak seems like a good option, and it also lets ppl use Oauth stuff like Google or Github, or just sign up with email and password
  - Using CockroachDB for scaling reasons (and bc open source, and free for small numbers of users)

- End to end encrypted chat
  - Important for security reasons
  - Doesn't make sense to implement this from scratch, should use a trusted implementation
  - end to end encrypted voice chat seems harder bc encryption in real time is hard? (idk tho)
  - but text should be doable
  - While Signal is good, doesn't easily support self hosting
  - Matrix seems like a good option, in particular conduwuit for performance reasons
  - However, conduwuit doesn't connect to a seperate DB, it holds the DB in a rocksdb locally
      - This requires persistant storage, so free tier of hosting on render doesn't work
      - Users could host it themselves, however would require getting conduwuit working on windows (might be possible, or even easy idk yet)
      - Might be possible to attach to cockroachdb? (since it was built on rocksdb) but doesn't seem supported by default so would be a bit of work
  - How does conduwuit do auth? Can I attach it to keycloak?

- Storage of user inventories and worlds:
  - IPFS
    - Stored locally on the user's device, so no cloud backup
    - Other users can persist data when fetched, but by default it may get removed if not pinned
    - The main advantage is that commonly used files get persisted across the network
    - How hard would search of all "public" inventories be?
    - Could offer optional cloud backup options (including set up yourself options) in addition to IPFS
    - The number of problems actually solved by this isn't very many, rolling our own could be fine
    - Seems annoying for sharing items (say I save someone else's folder, then they go offline)
       - Either we need to locally save all files in any folder saved (requires lots of data transfer, and requires
          ensuring the files get properly updated when the original user modifies things
            the version issue can be addressed by holding a version of the public folder and
            having a ui for updating to the most recent when it's made visible (IPFS has nice version control options).
            The main issue is needing to copy the whole folder's contents, requires the other user to be online and
            use their bandwith for that transfer)
       - Or we need to accept that things can't be accessed when the original user isn't online
         - I don't like this and it would be confusing and annoying for users
       - Both options aren't great, seems better to have a federated model where
         - Users have a host "server" that is expected to always be online, and stores their inventory contents
         - "autohost while playing" is discouraged as it'll have downtime when they stop playing,
            - instead, these are seperate servers expected to always be up
         - It's actually fine to have a user's inventory go up and down, the important thing is that any folders that you share
            should have a host server they belong to
            - However having to specify the host server of every folder you make to share it sounds confusing
              - good to have it as an option, however that requires the user to authenticate themselves with that server
              - If they have to go through the whole auth flow, makes more sense to just give them a "home server" that they belong to
              - The option to switch which server you are hosting various content on seems confusing, better to just give them an
                  easy way to switch accounts to accounts on other servers
                  - How would this work?
         - Actually an option I like is that your root inventory has a folder for every server that you belong to
           - You can belong to multiple servers, each has a seperate inventory you can store stuff in
           - Should have easy migration options, very important
           - There's also a local inventory option
           - Stuff saved on a server isn't by default public, you have to mark it as "shared"
              - Then you can share it with others and they can save it
                - This has the issue of they can't save it inside their inventory if it's not the same server
                - Trying to do that sort of saving should probably transfer it to your server by default?
                  - Not great, as it could get desynced
                  - Would be nice if a server could store content that is just linked to another server
                  - Could be nice if it creates a folder that's just a link to stuff you have saved on that server
                    - But might be confusing: should it be in the base level? Or should it mirror the entire folder structure you have?
                       - Both would be confusing to users
                  - Saying "You can't save this, navigate to their server to save it" sounds annoying and confusing and bad user experience
                  - Hmm will need to think on this
         - All folders also have a host server
  - CockroachDB
    - Scales better but requires users to setup their own CockroachDB or use a federated one
  - Probably makes sense to provide both options

- Connecting users
    - We can use a STUN server, and TURN server (forward traffic through server) for fallback 
    - This should scale alright, ideally users spin up their own server if they are frequently needing to forward traffic

- Voice chat
    - Once we have a connection via STUN/TURN this is simple and there's plenty of good implmentations of voice codecs





General infra:
- User inventory stored locally initially
  - Steam cloud sync lets it be saved
  - Sharing stuff just copies contents
- Worlds are hosted by a specific user (or headless)
  - Anyone else P2P connects to them
- Worlds are discovered how?
  - When hosting a world, if make it public world you can choose for it to "belong" to an instance you are a part of
    - Anyone browing that instance (or instances they federate with) can then see that instance as public
  - Anyone in your contacts?
  - TODO: more thinking
  - The purpose of instances is only for this discoverability
    - Instances also 


CockroachDB seems like a good option for scaling the server to many users

Matrix (in particular, conduwuit) seems like a good option for 