<p align="center"><a href="https://dataease.io"><img src="https://hoey-images.oss-cn-hangzhou.aliyuncs.com/img/MoviePilot-logo.jpeg" alt="DataEase" width="300" /></a></p>
<h3 align="center">MoviePilot</h3>
<p align="center">
  <a href="https://www.gnu.org/licenses/gpl-3.0.html"><img src="https://img.shields.io/github/license/dataease/dataease?color=%231890FF" alt="License: GPL v3"></a>
  <a href="https://app.codacy.com/gh/dataease/dataease?utm_source=github.com&utm_medium=referral&utm_content=dataease/dataease&utm_campaign=Badge_Grade_Dashboard"><img src="https://app.codacy.com/project/badge/Grade/da67574fd82b473992781d1386b937ef" alt="Codacy"></a>
  <a href="https://github.com/dataease/dataease"><img src="https://img.shields.io/github/stars/dataease/dataease?color=%231890FF&style=flat-square" alt="Stars"></a>
</p>





<p align="center'>
<a href="./README.md">English</a>|<a href="readme/README.zh_CN.md">简体中文</a>
</p>


# MoviePilot

Based on some code refactoring of [NAStool](https://github.com/NAStool/nas-tools), MoviePilot focuses on automating core needs, reducing issues, and making it easier to expand and maintain.

# For learning and communication purposes only, do not promote this project on any domestic platforms!

Release channel: https://t.me/moviepilot_channel

## Key Features
- Front and back end separation, based on FastApi + Vue3. Front-end project address: [MoviePilot-Frontend](https://github.com/jxxghp/MoviePilot-Frontend), API: http://localhost:3001/docs
- Focus on core needs, simplify functions and settings, some settings can be used with default values.
- Redesigned user interface for improved aesthetics and usability.

## Installation

### Note: Admin users should not use weak passwords! If not necessary, do not expose it to the public network. If the management account permissions are stolen, it will result in sensitive data leakage such as site cookies!

### 1. **Install the CookieCloud plugin**

Site information needs to be obtained through CookieCloud synchronization. Therefore, you need to install the CookieCloud plugin to synchronize site cookie data from the browser to the cloud before synchronizing to MoviePilot. Click [here](https://github.com/easychen/CookieCloud/releases) to download the plugin.

### 2. **Install CookieCloud server (optional)**

MoviePilot comes with a public CookieCloud server. If you need to build your own service, refer to the [CookieCloud](https://github.com/easychen/CookieCloud) project for setup. Click [here](https://hub.docker.com/r/easychen/cookiecloud) for the Docker image.

**Disclaimer:** This project does not collect user sensitive data. Cookie synchronization is also based on the CookieCloud project and not a capability provided by this project. Technically, CookieCloud uses end-to-end encryption, and third parties cannot steal any user information (including server owners) without leaking the `user KEY` and `end-to-end encryption password`. If you are not confident, you can choose not to use the public service or this project. However, any information leakage that occurs after usage is not related to this project!

### 3. **Install accompanying management software**

MoviePilot requires a downloader and media server for use.
- Downloader support: qBittorrent, Transmission, QB version >= 4.3.9 required, TR version >= 3.0 required, QB recommended.
- Media server support: Jellyfin, Emby, Plex, Emby recommended.

### 4. **Install MoviePilot**

- Docker image

  Click [here](https://hub.docker.com/r/jxxghp/moviepilot) or run the command:

  ```shell
  docker pull jxxghp/moviepilot:latest
  ```

- Windows

  Download [MoviePilot.exe](https://github.com/jxxghp/MoviePilot/releases), double-click to run and generate the configuration file directory, then visit: http://localhost:3000

- Synology suite

  Add suite source: https://spk7.imnks.com/

- Local deployment

  1) Copy all files under the directory [MoviePilot-Plugins](https://github.com/jxxghp/MoviePilot-Plugins) to the `app/plugins` directory.
  2) Copy all files under the directory [MoviePilot-Resources](https://github.com/jxxghp/MoviePilot-Resources) to the `app/helper` directory.
  3) Run the command: `pip install -r requirements.txt` to install dependencies.
  4) Run the command: `PYTHONPATH=. python app/main.py` to start the service.
  5) Follow the instructions of the front-end project [MoviePilot-Frontend](https://github.com/jxxghp/MoviePilot-Frontend) to start the frontend service.

## Configuration

Most configurations can be set through the WEB management interface after startup, but some configurations still need to be set through environment variables/configuration files.

Configuration file mapping path: `/config`, configuration item effective priority: environment variables > env file (or through WEB interface configuration) > default value.

> Items marked with an exclamation mark are required fields, others are optional. Optional items can be deleted to use default values.

### 1. **Environment variables**

- **❗NGINX_PORT:** WEB service port, default `3000`, can be modified, must not conflict with the API service port.
- **❗PORT:** API service port, default `3001`, can be modified, must not conflict with the WEB service port.
- **PUID:** UID of the user running the program, default `0`.
- **PGID:** GID of the user running the program, default `0`.
- **UMASK:** Mask permission, default `000`, consider setting it to `022`.
- **PROXY_HOST:** Network proxy, access to themoviedb or restart updates requires proxy access, format: `http(s)://ip:port`, `socks5://user:pass@host:port`.
- **MOVIEPILOT_AUTO_UPDATE:** Automatically update on restart, `true`/`release`/`dev`/`false`, default `release`, must be able to connect to Github **Note: If there are network issues, configure `PROXY_HOST`.**
- **❗AUTH_SITE:** Authentication site (must be authenticated before using site-related functions), supports configuring multiple authentication sites, separated by `,`, such as: `iyuu,hhclub`, will execute authentication operations in sequence until one site is successfully authenticated.
  
    After configuring `AUTH_SITE`, you need to configure the corresponding authentication parameters for the site according to the table below.
    Supported by authentication resource `v1.1.4`: `iyuu` / `hhclub` / `audiences` / `hddolby` / `zmpt` / `freefarm` / `hdfans` / `wintersakura` / `leaves` / `ptba` / `icc2022` / `ptlsp` / `xingtan` / `ptvicomo` / `agsvpt` / `hdkyl`
  
    | Site Name | Parameters |
    |:--------:|:---------:|
    | iyuu | `IYUU_SIGN`: IYUU login token |
    | hhclub |`HHCLUB_USERNAME`: username<br/>`HHCLUB_PASSKEY`: key |
    | audiences |`AUDIENCES_UID`: user ID<br/>`AUDIENCES_PASSKEY`: key |
    | hddolby |`HDDOLBY_ID`: user ID<br/>`HDDOLBY_PASSKEY`: key |
    | zmpt |`ZMPT_UID`: user ID<br/>`ZMPT_PASSKEY`: key |
    | freefarm |`FREEFARM_UID`: user ID<br/>`FREEFARM_PASSKEY`: key |
    | hdfans |`HDFANS_UID`: user ID<br/>`HDFANS_PASSKEY`: key |
    | wintersakura |`WINTERSAKURA_UID`: user ID<br/>`WINTERSAKURA_PASSKEY`: key |
    | leaves |`LEAVES_UID`: user ID<br/>`LEAVES_PASSKEY`: key |
    | ptba |`PTBA_UID`: user ID<br/>`PTBA_PASSKEY`: key |
    | icc2022 |`ICC2022_UID`: user ID<br/>`ICC2022_PASSKEY`: key |
    | ptlsp |`PTLSP_UID`: user ID<br/>`PTLSP_PASSKEY`: key |
    | xingtan |`XINGTAN_UID`: user ID<br/>`XINGTAN_PASSKEY`: key |
    | ptvicomo |`PTVICOMO_UID`: user ID<br/>`PTVICOMO_PASSKEY`: key |
    | agsvpt |`AGSVPT_UID`: user ID<br/>`AGSVPT_PASSKEY`: key |
    | hdkyl |`HDKYL_UID`: user ID<br/>`HDKYL_PASSKEY`: key |

### 2. **Environment variables / Configuration files**

Configuration file name: `app.env`, place the configuration file in the root directory.

- **❗SUPERUSER:** Superuser username, default `admin`, use this user to log in to the admin interface after installation. **Note: The initial password for the superuser user is auto-generated. You need to check it in the log of the first run! If the first run log is lost, you need to delete the `user.db` file in the configuration file directory and then restart the service.**
- **❗API_TOKEN:** API key, default `moviepilot`, add `?token=` with this value in media server webhook, WeChat callback, and other address settings that require it. It is recommended to change it to a complex string.
- **BIG_MEMORY_MODE:** Large memory mode, default `false`, enabling it will increase the number of caches, occupying more memory but improving response speed.
- **META_CACHE_EXPIRE:** Metadata recognition cache expiration time (in hours), numeric value, if not configured or `0`, will use the system default (7 days for large memory mode, 3 days otherwise), increasing this value can reduce the number of themoviedb accesses.
- **GITHUB_TOKEN:** Github token, improves Github Api request rate limit threshold for auto-updates, plugin installation, etc., format: `ghp_****`.
- **DEV:** Developer mode, `true`/`false`, default `false`, enabling it will pause all scheduled tasks.
- **AUTO_UPDATE_RESOURCE:** Automatically check and update resource packages (site index and authentication, etc.) on startup, `true`/`false`, default `true`, needs a valid Github connection, only supports Docker images.
---
- **TMDB_API_DOMAIN:** TMDB API address, default `api.themoviedb.org`, can also be configured as `api.tmdb.org`, `tmdb.movie-pilot.org`, or other intermediate proxy service addresses as long as they are reachable.
- **TMDB_IMAGE_DOMAIN:** TMDB image address, default `image.tmdb.org`, can be configured to other intermediate proxies to speed up TMDB image display, such as: `static-mdb.v.geilijiasu.com`.
- **WALLPAPER:** Movie poster on the login homepage, `tmdb`/`bing`, default `tmdb`.
- **RECOGNIZE_SOURCE:** Media information recognition source, `themoviedb`/`douban`, default `themoviedb`, using `douban` does not support secondary classifications.
- **FANART_ENABLE:** Fanart switch, `true`/`false`, default `true`, turning it off will significantly reduce the types of scrapped images.
- **SCRAP_SOURCE:** Data source used for scraping metadata and images, `themoviedb`/`douban`, default `themoviedb`.
- **SCRAP_FOLLOW_TMDB:** Whether to follow TMDB information changes for newly added media, `true`/`false`, default `true`. When set to `false`, even if TMDB information changes, it will still scrape based on the information already added to the history records.
---
- **AUTO_DOWNLOAD_USER:** User ID for automatic optimal download during remote interaction search (user ID for message notification channel), multiple users separated by `,`, setting as `all` means automatically select optimal download for all users. If not set, you need to manually select resources or reply `0` for automatic optimal download.
---
- **OCR_HOST:** OCR recognition server address, format: `http(s)://ip:port`, used for recognizing site captchas for automatic login and cookie retrieval, not configured will default to using the built-in server `https://movie-pilot.org`, a [Docker image](https://hub.docker.com/r/jxxghp/moviepilot-ocr) can be self-deployed.
---
- **DOWNLOAD_SUBTITLE:** Download site subtitles, `true`/`false`, default `true`.
---
- **MOVIE_RENAME_FORMAT:** Movie renaming format, based on jinjia2 syntax.

  Configurable items supported by `MOVIE_RENAME_FORMAT`:

  > `title`: Title from TMDB/Douban  
  > `en_title`: English title from TMDB (Douban not supported yet)  
  > `original_title`: Original language title from TMDB/Douban  
  > `name`: Name identified from file name (prefers Chinese when both Chinese and English present)  
  > `en_name`: English name identified from file name (can be empty)  
  > `original_name`: Original file name (including file extension)  
  > `year`: Year  
  > `resourceType`: Resource type  
  > `effect`: Effect  
  > `edition`: Version (resource type + effect)  
  > `videoFormat`: Resolution  
  > `releaseGroup`: Production/subtitle group  
  > `customization`: Custom placeholder  
  > `videoCodec`: Video codec  
  > `audioCodec`: Audio codec  
  > `tmdbid`: TMDB ID (empty for non-TMDB recognized sources)  
  > `imdbid`: IMDB ID (may be empty)  
  > `doubanid`: Douban ID (empty for non-Douban recognized sources)  
  > `part`: Segment/section  
  > `fileExt`: File extension  
  > `customization`: Custom placeholder

  Default configuration format for `MOVIE_RENAME_FORMAT`:
  
  ```
  {{title}}{% if year %} ({{year}}){% endif %}/{{title}}{% if year %} ({{year}}){% endif %}{% if part %}-{{part}}{% endif %}{% if videoFormat %} - {{videoFormat}}{% endif %}{{fileExt}}
  ```

- **TV_RENAME_FORMAT:** TV show renaming format, based on jinjia2 syntax

  Additional configuration items supported by `TV_RENAME_FORMAT`:

  > `season`: Season number  
  > `episode`: Episode number  
  > `season_episode`: Season and episode SxxExx  
  > `episode_title`: Episode title

  Default configuration format for `TV_RENAME_FORMAT`:
  
  ```
  {{title}}{% if year %} ({{year}}){% endif %}/Season {{season}}/{{title}} - {{season_episode}}{% if part %}-{{part}}{% endif %}{% if episode %} - Episode {{episode}}{% endif %}{{fileExt}}
  ```

### 3. **Priority Rules**

- Only supports arranging combinations using built-in rules to achieve priority sequence matching.
- Resources that meet any level of rules will be identified as selected, and the matched level will be the priority level for the resource.
- Resources that do not meet the filtering rules at any level will not be selected.

### 4. **Plugin Extension**

- **PLUGIN_MARKET:** Plugin market repository address, only supports Github repositories with the `main` branch, multiple addresses separated by `,`, default to the official plugin repository: `https://github.com/jxxghp/MoviePilot-Plugins`, for more third-party plugin repositories, check the fork of the [MoviePilot-Plugins](https://github.com/jxxghp/MoviePilot-Plugins) project or refer to the pinned messages in the channel.

## Usage

### 1. **WEB Admin Panel**
- Log in to the admin panel with the set superuser account (``SUPERUSER` configuration item, default user: admin, default port: 3000).
> **Note: The initial password for the superuser user is auto-generated. You need to check it in the log of the first run! If the first run log is lost, you need to delete the `user.db` file in the configuration file directory and then restart the service.
### 2. **Site Maintenance**
- Quickly add sites through CookieCloud synchronization; disable or delete sites not in use via the WEB management interface; manually add sites that cannot be synchronized.
- Site-related functions can only be used after setting user authentication information through environment variables and successfully passing authentication. Plugins related to the site will also not be displayed if authentication fails.
### 3. **File Organization**
- Enable `Downloader Monitoring` switch in the background to automatically organize and scrape media information into the library after downloads complete; it will only process tasks added to downloads through MoviePilot.
- Use `Directory Monitoring` and other plugins for more flexible automatic organization.
### 4. **Notification Interaction**
- Manage and subscribe to downloads remotely via `WeChat`/`Telegram`/`Slack`/`SynologyChat`/`VoceChat` channels; WeChat/Telegram will automatically add operation menus (limitations on WeChat menu count, some menus may not be displayed).
- Callback addresses for `WeChat`, `SynologyChat`, relative paths are: `/api/v1/message/`; `VoceChat` webhook address relative path is: `/api/v1/message/?token=moviepilot`, where `moviepilot` is the set `API_TOKEN`.
### 5. **Subscription and Search**
- Search and subscribe via the MoviePilot admin panel.
- Add MoviePilot as a server to `Overseerr` or `Jellyseerr` for browsing and subscribing with `Overseerr/Jellyseerr`.
- Install plugins like `Douban List Subscription`, `Cat Eye Subscription`, etc., to automatically subscribe to Douban lists, Cat Eye lists, etc.
### 6. **Others**
- Enable playing notifications via MoviePilot by setting media server webhooks directed to MoviePilot (`/api/v1/webhook?token=moviepilot`, where `moviepilot` is the set `API_TOKEN`), and pairing with various plugins to implement playback restrictions, etc.
- Map the host `docker.sock` file to the container `/var/run/docker.sock` to support built-in restart operations. Example: `-v /var/run/docker.sock:/var/run/docker.sock:ro`.
- Add the WEB page to the mobile desktop for a similar app-like experience.

### **Caution**
- The container requires the first start to download the browser kernel, which may take a long time depending on the network, preventing login during this time. You can map the `/moviepilot` directory to avoid triggering the browser kernel download when the container is reset.
- When using a reverse proxy, add the following configuration; otherwise, some functions may become inaccessible (modify `ip:port` to actual values):
```nginx configuration
location / {
    proxy_pass http://ip:port;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```
- When using a reverse proxy with SSL, enable `http2`, otherwise it may cause long loading times or unavailability. Example using `Nginx`:
```nginx configuration
server {
    listen 443 ssl;
    http2 on;
    ...
}
```
- Newly created WeChat enterprise app requires a proxy with fixed public IP to receive messages; the
