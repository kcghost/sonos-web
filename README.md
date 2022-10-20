# sonos-web
Sonos Web is a browser based controller for your [Sonos](https://www.sonos.com/system) sound system.

Install Sonos Web on a single computer and access and manage your system from any browser on your network.

![Search View](https://user-images.githubusercontent.com/5977736/51435364-fec86800-1c32-11e9-8ca0-a162b1dc1e91.png)


Sonos Web relies upon the [node-sonos](https://github.com/bencevans/node-sonos) project for all its Sonos communication.

Sonos Web has been tested on Windows 10, macOS, Linux, and Raspbian Stretch Lite (9.6).

[Check out the following Youtube video](https://youtu.be/0q8Z-XV81Z4) to see a brief demo of the system in use. (v0.5.2)

## Oh Fork it's a Fork!

This is a fork of [Villarrealized's excellent project](https://github.com/Villarrealized/sonos-web), ~~which appears to be abandoned~~.
The [main sonos-web project](https://github.com/sonos-web/sonos-web) is under active development again and has a working Docker image, you should probably use that instead of this personal fork!

Several fixes were pulled into a branch by [stufisher](https://github.com/stufisher/sonos-web/tree/update_and_fix) which this fork uses.

When I attempted to get this project working for myself I found that:
* Docker images on Dockerhub that use sonos-web are based off the original project, and thus are broken.
* The installation method [sonos-web-cli](https://github.com/Villarrealized/sonos-web-cli) is of course tied to the original project and is abit infuriating to dissect what its doing
* It's non-obvious how to actually run the project from sources (at least for a node newbie?)
* Even when I got it running with fixes, I still needed to fix an error from [node-sonos](https://github.com/bencevans/node-sonos) to get things running.

So I have added a Makefile that builds and runs a server right from these sources, no sonos-web-cli necessary. Just do `make serve`.
Right now this project depends on a fork of node-sonos, but I submitted a pull request to get the fixes upstream.

I have also added a [Docker image to Dockerhub](https://hub.docker.com/r/kcghst/sonos-web) to make the project very convenient for Linux Docker users.

## Build and run from sources

```
make serve
```

## Docker Support

A minimal pre-built Docker image is available on [Dockerhub](https://hub.docker.com/r/kcghst/sonos-web).
Only Linux is currently supported, as you need [Docker's host networking feature](https://docs.docker.com/network/host/) for Sonos device discovery. The actual web server uses port 5050 by default.

From CLI or script:
```
docker run -it --net=host -e PORT=5050 kcghst/sonos-web
```

docker-compose.yml:
```
  sonos:
    container_name: sonos
    image: kcghst/sonos-web:latest
    network_mode: "host"
	environment:
	  - PORT=5050
```

nginx.conf proxy:
```
	server {
		server_name sonos.*;

		location / {
            proxy_pass http://$hostip:5050;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Protocol $scheme;
            proxy_set_header X-Forwarded-Host $http_host;
		}
	}
```

## TODO?

* It might be possible to avoid host networking by allowing manual pre-configuration of Sonos hosts
* The Music Library doesn't work for me personally, I think it requires the Windows share?
* Would be nice if it could browse music from a DLNA server, even nicer if it had access to everything the Sonos app does

## Features
 * Music Library
    * Search your entire music library with Top Results *
      * Including Sonos Playlists! *
    * Browse, Queue, & Play your entire music library by category
    * View Artists, Genres & Playlists with Album Art! *
    * All artist & album names are links that take you to that item *
    * Browsing & searching is cached for quicker load times
 * Sonos Playlists & Favorites
 * Spotify
    * Browse & play your saved spotify playlists, albums, & songs
    * Requires Spotify Premium to play songs
 * Group rooms together via drag & drop *
 * Queue management via icon on Now Playing bar
 * Party mode (Group all speakers into one group)
 * Quickly & easily switch between rooms/groups
 * Adjust individual speaker volume/mute or adjust for the whole group 
 * Play, pause, skip, and seek tracks
 * Shuffle & repeat controls 
 * Room selection from the Now Playing bar.

 \* *Feature exclusive to Sonos Web (not available on official Sonos App)*
 

## Planned Features
 * Music Library    
    * Create & Manage Sonos Playlists & Favorites
 * Integration with other Music Services
    * Spotify (added in v0.9.0)
      * Allow searching of Spotify
 * Recent Play History

## Screenshots

### Now Playing
![Now Playing](https://user-images.githubusercontent.com/5977736/52979809-91743800-3394-11e9-9566-f8d72a951fa8.png)

### Queue Management
![Queue Management](https://user-images.githubusercontent.com/5977736/50566855-89d0d500-0cfb-11e9-8c7c-181a624b5eb7.png)

### Search View
![Search View 2](https://user-images.githubusercontent.com/5977736/51435372-4ea72f00-1c33-11e9-9cdc-b7adf47e9d86.png)

### Album Detail View
![Album View](https://user-images.githubusercontent.com/5977736/51081850-4b51f780-16b7-11e9-88a4-4466a9af640d.png)

### Playlists View
![Playlists View](https://user-images.githubusercontent.com/5977736/51081853-602e8b00-16b7-11e9-9944-91384dd74f17.png)

### Songs View
![Songs View](https://user-images.githubusercontent.com/5977736/51081854-60c72180-16b7-11e9-8197-8f2080ad253a.png)

### Genre Detail View
![Genre Detail View](https://user-images.githubusercontent.com/5977736/51081846-09c14c80-16b7-11e9-8fd8-a158d1a1b8f6.png)

### Sonos Favorites & Playlists
![Sonos Favorites & Playlists](https://user-images.githubusercontent.com/5977736/52084269-e889a880-255e-11e9-96db-d73861e6b178.png)

### Spotify Playlists
![Spotify Playlists](https://user-images.githubusercontent.com/5977736/52979583-7228db00-3393-11e9-9555-395bcf2b8df1.png)

### Spotify Playlist Detail
![Spotify Playlist Detail](https://user-images.githubusercontent.com/5977736/52979580-6fc68100-3393-11e9-8f9b-206d401c12b4.png)

### View all rooms and what they're playing
![Rooms](https://user-images.githubusercontent.com/5977736/50566842-5c842700-0cfb-11e9-8e7b-56a981769d26.png)

### Adjust volume as a group or as individual rooms
![Grouped Rooms & Individual Volume Adjustment](https://user-images.githubusercontent.com/5977736/50566804-fdbead80-0cfa-11e9-86c9-21290ff33288.png)

## License
[GPL v3.0](LICENSE)

