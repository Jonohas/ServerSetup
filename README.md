# ServerSetup
This is the setup guide to a docker only debian server
## Install docker

[docker](https://docs.docker.com/engine/install/debian/) & [docker-compose](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-debian-10)


##### Create network for all managed websites
```
docker network create -d bridge websites
```
##### Install Nginx Proxy Manager

```py linenums="1"
version: "3"
services:
  app:
    container_name: service_npm
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of 
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    networks:
      - websites

networks:
  websites:
    external: true
```
then
```
docker-compose up -d
```

