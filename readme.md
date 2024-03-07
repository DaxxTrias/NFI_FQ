# Freqtrade & NostalgiaForInfinityX

This github repository is currently targeting OKX with 4 different bot strategies.

## Pre-requisites

* [Docker](https://www.docker.com/products/docker-desktop/)
  * [Docker-Compose](https://docs.docker.com/compose/compose-file/)

* [Python](https://www.python.org/downloads/)

* [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) if using Windows

- - - -

## Installation

Install **Docker** and **Python**

* if you have difficulties installing Docker on Windows it might be related to your Hypervisor not being enabled.
  * You can [read more here](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

Once you have Python installed make sure pip is also installed

```shell
pip --version
```

This should provide you feedback indicating a version # if its installed properly.
If its not installed you can go [here to get it](https://pip.pypa.io/en/stable/installation/)

Once this is done it should be as simple as executing the docker launch

```shell
docker-compose up
```

If this does not work you should verify docker-compose is installed and setup

* addtl resources here for [docker-compose](https://dockerlabs.collabnix.com/intermediate/workshop/DockerCompose/How_to_Install_Docker_Compose.html)

* If using Linux [check this out](https://docs.docker.com/engine/install/linux-postinstall/)

if you do not want the console to be captured for the duration of its runtime you can instead launch it with the daemon parameter:

```shell
docker-compose up -d
```

- - - -

### Notes

* Provided you are fine with running this under **Docker**, you do not need to install the normal requirements to run Freqtrade since docker will automate it for you courtesy of other [files](../master/docker/Dockerfile.custom) in this repo.
* Please see **requirements.txt** file in the main [Freqtrade Repo](https://github.com/freqtrade/freqtrade/blob/develop/requirements.txt) if you wish to run this outside of **Docker**.
* Please make sure you have Docker Version 1.13 or newer or you might not be able to run this docker compose file properly. [Read more here](https://docs.docker.com/compose/compose-file/compose-versioning/#version-13)

- - - -

## General Configuration

Be sure to edit the **docker-compose.yml** file for your specific situation. I have included some sample files to get you started however it defaults to OKX, so you will need to adjust this yourself for other exchanges. It also won't work out of the box since I did not include a ***private.json** file, read more about that below.

<div align="center">

> Docker-Compose.yml
</div>

```YAML
      --logfile user_data/logs/nfi-okx.log
      --db-url sqlite:////freqtrade/user_data/nfi-okx.sqlite
      --datadir user_data/data/${EXCHANGE:-OKX}
      --config user_data/data/okx-usdt-private.json
      --config configs/pairlist-volume-okx-usdt.json
      --config configs/blacklist-okx.json
      --strategy NostalgiaForInfinityX
```

* Ideally you would want a custom config setup for each of the exchanges you intend to use the bot on or have multiple different 'services' in the docker file
  * lines 3-7 above are responsible for the bulk of inter-exchange operability

- - - -

## Specific Exchange Config

There are generally 3 files that change from exchange to exchange (these are my names, you dont have to use my names you should get creative)

```txt
* okx-usdt-private.json
* pairlist-volume-okx-usdt.json
* blacklist-okx.json
```

In this above example the most important file is the **private.json** file which houses the core configuration setup. Inside it are various settings which need to be modified on a per-user & per-bot basis. I **do not** provide you with a **private json file**, you will need to take a public file and make a copy of it and name it private, or edit the **docker-compose.yml** file to point to the public file instead.

<div align="center">

> okx-usdt-private.json
</div>

```YAML
  "exchange": {
      "name": "okx",
      "key": "yourkey",
      "secret": "yoursecret",
```

The most important section is this one. you need to set the appropriate name for your exchange. **binance** || **binanceus** || **kucoin** || **bybit** || **okx** etc

If using a dry-run (which is what **this repo defaults to**) you do not need to supply a key & secret, since its all just pretend anyway. The keys are needed only if you want the bot to actually trade with real funds. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/exchanges.md)

<div align="center">

> okx-usdt-private.json - Telegram Section
</div>

```YAML
  "telegram": {
    "enabled": false,
    "token": "yourtoken",
    "chat_id": "yourchatid",
```

In this section you would change enabled to true and provide a token and id code for your telegram bot if you wish to use the Telegram Bot interface for controlling your bot remotely. **Token** would be the unique identifier of the telegram bot itself when you create the bot with **Botfather**, **chatid** would be your own personal telegram UID#. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/telegram-usage.md)

<div align="center">

> okx-usdt-private.json - FreqUI interface
</div>

```YAML
  "api_server": {
    "enabled": true,
    "listen_ip_address": "0.0.0.0",
    "listen_port": 8070,
    "verbosity": "error",
    "enable_openapi": false,
    "jwt_secret_key": "yoursecret",
    "CORS_origins": ["http://localhost:8070"],
    "username": "yourusername",
    "password": "yourpassword"
```

* If you have issues with the API server not working after filling out the settings try **uncommenting** the two lines relating to ports in the **docker-compose.yml** file

In this section you can change settings related to the WEB API so you can monitor from the browser dashboard utilizing [FreqUI](https://github.com/freqtrade/freqtrade/blob/develop/docs/rest-api.md)

* The important settings to change here are the **listen_port**, **jwt_secret_key**, and the **username** & **password** fields
  * Be careful using 0.0.0.0 if hosting this on a public server, you might want to change this to a specific IP address instead.

* If running multiple bots you want them to all point to the same CORS Origin so you only have to visit 1 browser address to manage all of them

- - - -

## Docker Documentation & other Guides

The following guides / sources can get you started with additional learning materials related to Docker & Freqtrade (To assist you with doing customizations)

<div align="center">

[Quickstart](https://www.freqtrade.io/en/stable/docker_quickstart/)

[Rest API for Web](https://www.freqtrade.io/en/latest/rest-api/)

[Telegram](https://www.freqtrade.io/en/latest/telegram-usage/)

[Freqtrade](https://www.freqtrade.io/en/stable/)

[Nostalgia For Infinity X](https://github.com/iterativv/NostalgiaForInfinity)

[NFI WIKI for quickly getting setup](https://github.com/iterativv/NostalgiaForInfinity/wiki)

[Docker and WSL2 vs Hyper-V](https://docs.docker.com/desktop/windows/wsl/)

</div>
