# Freqtrade & NostalgiaForInfinityX

This github repository is currently targeting a single Freqtrade Bot with NFI. Will upload an alternate setup for multi-bots later

## Pre-requisites

* [Docker](https://www.docker.com/products/docker-desktop/)
  * [Docker-Compose](https://docs.docker.com/compose/compose-file/)

* [Python](https://www.python.org/downloads/)

* [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) if using Windows


## Installation

Once you have Python installed make sure pip is also installed

```
pip --version
```

This should provide you feedback indicating a version # if its installed properly.
If its not installed you can go [here for addtl help](https://pip.pypa.io/en/stable/installation/)

Once this is done it should be as simple as executing the docker launch

```
docker-compose up
```

If this does not work you should verify docker-compose is installed and setup

* addtl resources here for [docker-compose](https://dockerlabs.collabnix.com/intermediate/workshop/DockerCompose/How_to_Install_Docker_Compose.html)

* If using Linux [check this out](https://docs.docker.com/engine/install/linux-postinstall/)


if you do not want the console to be captured for the duration of its runtime you can instead launch it with the daemon parameter:

```
docker-compose up -d
```

### Notes

Provided you are fine with running this under **Docker**, you do not need to install the normal requirements to run Freqtrade since docker will automate it for you courtesy of other [files](../docker/docker/Dockerfile.custom) in this repo. Please see the requirements.txt file in the main [Freqtrade Repo](https://github.com/freqtrade/freqtrade/blob/develop/requirements.txt) if you wish to run this outside of **Docker**.

## General Configuration

Be sure to edit the **docker-compose.yml** file for your specific situation. I have included some sample files to get you started however it defaults to Kucoin, so you will need to adjust this yourself for other exchanges. It also won't work out of the box since I did not include a ***private.json** file, read more about that below.

> Docker-Compose.yml
```YAML 
      --logfile user_data/logs/freqtrade.log
      --db-url sqlite:////freqtrade/user_data/tradesv3.sqlite
      --datadir user_data/data/${EXCHANGE:-kucoin}
      --config user_data/data/pairlists-usdt-private.json
      --config configs/pairlist-volume-kucoin-usdt.json
      --config configs/blacklist-kucoin.json
      --strategy NostalgiaForInfinityX
```

* In this example changing line 3 above to binanceus or bybit if you intend to dedicate to one of their specific exchanges.
* Ideally you would want a custom config setup for each of the exchanges you intend to use the bot on
  * lines 4-7 above are responsible for the bulk of inter-exchange operability

## Specific Exchange Config

There are generally 3 files that change from exchange to exchange (these are my names, you dont have to use my names you should get creative)

```
* pairlists-usd-private.json
* pairlist-volume-kucoin-usd.json
* blacklist-kucoin.json
```

In this above example the most important file is the **private.json** file which houses the core configuration setup. Inside it are various settings which need to be modified on a per-user & per-bot basis. I **do not** provide you with a **private json file**, you will need to take a public file and make a copy of it and name it private, or edit the **docker-compose.yml** file to point to the public one after you have made your edits.

<div align="center">

> pairlists-kucoin-usdt-private.json
</div>

```YAML
  "exchange": {
      "name": "kucoin",
      "key": "yourkey",
      "secret": "yoursecret",
```

The most important section is this one. you need to set the appropriate name for your exchange. **binance** vs **binanceus** vs **kucoin** vs **bybit** etc

If using a dry-run (which is what **this repo defaults to**) you do not need to supply a key & secret, since its all just pretend anyway. The keys are needed only if you want the bot to actually trade with real funds. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/exchanges.md)

<div align="center">

> pairlists-kucoin-usdt-private.json
</div>

```YAML
  "telegram": {
    "enabled": false,
    "token": "yourtoken",
    "chat_id": "yourchatid",
```

In this section you would change enabled to true and provide a token and id code for your telegram bot. Token would be the unique identifier of the bot itself when you create the bot, chatid would be your own personal identifier. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/telegram-usage.md)

<div align="center">

> pairlists-kucoin-usdt-private.json
</div>

```YAML
  "api_server": {
    "enabled": false,
    "listen_ip_address": "0.0.0.0",
    "listen_port": 8070,
    "verbosity": "error",
    "enable_openapi": false,
    "jwt_secret_key": "yoursecret",
    "CORS_origins": ["http://localhost:8070"],
    "username": "yourusername",
    "password": "yourpassword"
```

* If you have issues with the API server not working after filling out settings try changing **listen_ip_address** to 127.0.0.1 some pc setups demand it this way for security reasons

In this section you can change settings related to the WEB API so you can monitor from the browser dashboard utilizing [FreqUI](https://github.com/freqtrade/freqtrade/blob/develop/docs/rest-api.md)

* The important settings to change here are the **listen_port**, **jwt_secret_key**, and the **username** & **password** fields

* If running multiple bots you want them to all point to the same CORS Origin so you only have to visit 1 browser address to manage all of them


## Docker Documentation & other Guides

The following guides are from Freqtrade and can get you started with additional learning materials related to Docker & Freqtrade (To assist you with doing customizations)

<div align="center">

[Quickstart](https://www.freqtrade.io/en/stable/docker_quickstart/)

[Rest API for Web](https://www.freqtrade.io/en/latest/rest-api/)

[Telegram](https://www.freqtrade.io/en/latest/telegram-usage/)

[Freqtrade](https://www.freqtrade.io/en/stable/)

[Nostalgia For Infinity X](https://github.com/iterativv/NostalgiaForInfinity)

[NFI WIKI for quickly getting setup](https://github.com/iterativv/NostalgiaForInfinity/wiki)
</div>