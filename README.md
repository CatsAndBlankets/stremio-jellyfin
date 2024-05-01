# Stremio Jellyfin Addon

[Stremio](https://www.stremio.com/) addon that enables streaming movies and TV series from your own Jellyfin server. Addon runs entirely locally, ensuring that none of your data is shared outside of your own network. It provides Stremio with a 'library' featuring your Jellyfin movies and TV series collection, allowing you to stream seamlessly both movies and series to your favorite Stremio player.

![](assets/si.png)

## Installation

This addon consists of two parts: Stremio Addon and supporting Jellyfin Extension adding Jellyfin search 
capability using IMDB identifiers. Both components are required.

### Jellyfin Stremio Companion Plugin

To install [it](https://github.com/akarazniewicz/jellyfin-providersid-search-plugin), simply add following, new addon repository to Jellyfin (`Jellyfin > Dashboard > Plugins > Repositories`):

https://raw.githubusercontent.com/akarazniewicz/jellyfin-providersid-search-plugin/main/manifest.json

You will have new plugin available in Jellyfin. Just activate 'Providers ID Items Search API' plugin.

![](assets/jp.png)

### Jellyfin Stremio Addon

Jellyfin Stremio addon should be installed in your local docker environment. To install it pull latest docker addon image:

`docker pull ghcr.io/akarazniewicz/stremio-jellyfin:latest`

If you would like to install this fork's docker image, you will need to clone the repository and build the image. This can be done by running this command inside the repositories' folder:
```
docker compose build t stremio-jellyfin:myversion .
```
Once the image is built, replace `ghcr.io/akarazniewicz/stremio-jellyfin:latest` with `stremio-jellyfin:myversion`


### Install Docker Container
Either run the container with docker:
```
docker run -d \
  --name stremio-jellyfin \
  --restart unless-stopped \
  -p 60421:60421 \
  -e JELLYFIN_USER="<your jellyfin username>" \
  -e JELLYFIN_PASSWORD="<your jellyfin user password>" \
  -e JELLYFIN_SERVER="<your jellyfin server address>" \
  ghcr.io/akarazniewicz/stremio-jellyfin:latest
```

Or use docker compose with a docker-compose.yaml file
```
services:
  stremio-jellyfin:
    image: ghcr.io/akarazniewicz/stremio-jellyfin:latest
    container_name: stremio-jellyfin
    restart: unless-stopped
    environment:
      - JELLYFIN_USER="<your jellyfin username>"
      - JELLYFIN_PASSWORD="<your jellyfin user password>"
      - JELLYFIN_SERVER="<your jellyfin server address>"
    ports:
      - 60421:60421
```

Note that:
* `60421` - is standard port addon is running on (You may remap it in docker)
* `<your jellyfin username>` - Jellyfin username
* `<your jellyfin user password>` - Jellyfin password
* `<your jellyfin server address>` - Jellyfin server address and port (`http://aaa.bbb.ccc.ddd:eee`). Make sure Jellyfin is connectable.

You can run it in Your docker orchestrator too (like Rancher or Unraid).

Finally, add the manifest to Stremio to install this addon:

`http://<your docker host>:60421/manifest.json`
