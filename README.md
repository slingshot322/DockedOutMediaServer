# DockedOutMediaServer
Includes the docker-compose files for all the media server essentials utilizing the following apps/images:

<ul>
  <li><a href="https://hub.docker.com/r/plexinc/pms-docker/">Plex (plexinc/pms-docker)</a></li>
  <li><a href="https://hub.docker.com/r/haugene/transmission-openvpn/">VPN/Transmission (haugene/transmission-openvpn)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/radarr/">Radarr (linuxserver/radarr)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/sonarr/">Sonarr (linuxserver/sonarr)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/lidarr/">Lidarr (linuxserver/lidarr)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/jackett/">Jackett (linuxserver/jackett)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/tautulli/">Tautulli (linuxserver/tautulli)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/ombi/">Ombi (linuxserver/ombi)</a></li>
  <li><a href="https://hub.docker.com/_/traefik/">Traefik (library/traefik)</a></li>
  <li><a href="https://hub.docker.com/r/linuxserver/muximux">Muximux (linuxserver/muximux)</a></li>
  <li><a href="https://hub.docker.com/_/nginx/">Nginx (library/nginx)</a>
    <ul>
      <li><a href="https://github.com/ITRav4/PlexRedirect">PlexRedirect<a></li>
    </ul>
  </li>
</ul>

<h1>Initial Setup</h1>
<ul>
    <li>Install <code>docker</code> (version minimum: <code>18.05.0-ce</code>) and <code>docker-compose</code> (version minimum: <code>1.21.2, build a133471</code>)</li>
    <li>Create a directory to clone this projects files into</li>
    <li>Rename the <code>.envblank</code> file to <code>.env</code> and modify to your specific setup</li>
</ul>

<h2>Create the Volumes:</h2>

<ul>
    <li><code>docker volume create --driver local --opt type=none --opt device=/path/to/media --opt o=bind media</code>
        <ul>
            <li><code>/path/to/media</code> is where all your media is store, likely where your TV/Movie subfolders are.</li>
        </ul>
    </li>
    <li><code>docker volume create --driver local --opt type=none --opt device=/path/to/music --opt o=bind music</code>
        <ul>
            <li><code>/path/to/music</code> is where all your music is stored, likely in subfolders by artist.</li>
        </ul>
    </li>
    <li><code>docker volume create --driver local --opt type=none --opt device=/path/to/downloaded --opt o=bind downloaded</code>
        <ul>
            <li><code>/path/to/downloaded</code> is where <code>transmission</code> will place all completed downloads. It's important this volume is created indepently as <code>sonarr/radarr/lidarr </code> will utilize the exact volume to monitor and pick up completed downloads.</li>           
        </ul>
    </li>
</ul>

<h2>Create the Network:</h2>

<ul>
    <li><code>docker network create --driver bridge --subnet 192.168.2.0/24 --ip-range 192.168.2.1/24 --gateway 192.168.2.1 media-network</code>
        <ul>
            <li>This step is necessary as the <code>docker-compose.override.yml</code> file is provided. However, omitting this step and removing the <code>media-network</code> will work too.</li>
        </ul>
    </li>
           
