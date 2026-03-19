# obsidian-sync 

> Service for synchronising edits to my Obsidian vault accross multiple machines 

Synchronisation is managed via a CouchDB database and the [LiveSync]() Obsidian community plugin. 
The provisioning and deployment of the DB is managed via Docker. See Docker
Compose file in this repo.

CouchDB runs on an HP T520 Thin Client (hostname: `hpt520-bravo`, local IP
address: 192.168.178,49) acting as
server on my local network. `hpt520-bravo` is node on my Tailscale network, as are all other devices that
want to Sync with the CouchDB server.

In order for LiveSync to work on mobile devices, the CouchDB instance must be
served from an HTTPS URL. This is a constraint of the LiveSync plugin. To achieve this,  I use the instance of nginx running on my remote
VPS to forward incoming HTTPS requests to
`https://obsidian-sync.systemsobscure.net` to the HTTP Tailscale address that is
running CouchDB on `hpt520-bravo` (100.xxx.xxx.xxx:5984). As the VPS is also a
node on the Tailscale network, it can forward requests to another node on this network.

Whenever I want to access the CouchDB instance from a device not on the
Tailscale network, I can just use the public HTTPS URL. For devices that are on
the Tailscale network I can also use the HTTPS URL _or_ the HTTP Tailscale URL so
long as it is not a mobile device.
