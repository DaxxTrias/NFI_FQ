# Freqtrade & NostalgiaForInfinityX

Source material can be found here

[Freqtrade](https://www.freqtrade.io/en/stable/)

[NFI](https://github.com/iterativv/NostalgiaForInfinity)

## Pre-requisites

* [Docker](https://www.docker.com/products/docker-desktop/)

* [Python](https://www.python.org/downloads/)

* [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) if using Windows


## Installation

Once you have Python installed make sure pip is also installed

```
pip --version
```

This should provide you feedback indicating a version # if its installed properly.
If its not installed you can go [here](https://pip.pypa.io/en/stable/installation/) for addtl help

Once this is done it should be as simple as executing the docker launch

```
docker-compose up
```

if you do not want the console to be captured for the duration of its run you can instead launch it with:

```
docker-compose up -d
```

### Notes

Provided you are fine with running this under docker, you do not need to install the normal requirements to run Freqtrade since docker will automate it for you courtesy of other files in this repo. Please see the requirements.txt file in the main [Freqtrade Repo](https://github.com/freqtrade/freqtrade/blob/develop/requirements.txt) if you wish to run this outside of **Docker**.

## General Configuration

Be sure to edit the **docker-compose.yml** file for your specific situation. I have included some sample files to get you started but it defaults to Kucoin for example, so you will need to adjust this yourself

```YAML
      --logfile user_data/logs/freqtrade.log
      --db-url sqlite:////freqtrade/user_data/tradesv3.sqlite
      --datadir user_data/data/${EXCHANGE:-kucoin}
      --config user_data/data/pairlists-usd-private.json
      --config configs/pairlist-volume-kucoin-usdt.json
      --config configs/blacklist-kucoin.json
      --strategy NostalgiaForInfinityX
```

* For example changing line 3 above to binanceus or bybit if you intend to dedicate to a specific exchange.
* Ideally you would want a custom config setup for each of the exchanges you intend to trade on

## Specific Exchange Config

There are generally 3 files that change from exchange to exchange (these are my names, you dont have to use my names get creative)

```
* pairlists-usd-private.json
* pairlist-volume-kucoin-usd.json
* blacklist-kucoin.json
```

In this above example the most important file is the **private.json** file which houses the core configuration setup. Inside it are various settings which need to be modified on a per-user per-bot basis. I **do not** provide you with a **private json file**, you will need to take a public file and make a copy of it named private, or edit the **docker-compose.yml** file to point to the public one.

```YAML
  "exchange": {
      "name": "binanceus",
      "key": "yourkey",
      "secret": "yoursecret",
```

The most important section is this one. you need to set the appropriate name for your exchange. **binance** vs **binanceus** vs **kucoin** vs **bybit** etc

If using a dry-run (which is what **this repo defaults to**) you do not need to supply a key & secret, since its all just pretend anyway. The keys are needed only if you want the bot to actually trade with real funds. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/exchanges.md)


```YAML
  "telegram": {
    "enabled": false,
    "token": "yourtoken",
    "chat_id": "yourchatid",
```

In this section you would change enabled to true and provide a token and id code for your telegram bot. Token would be the unique identifier of the bot itself when you create the bot, chatid would be your own personal identifier. [Read more here](https://github.com/freqtrade/freqtrade/blob/develop/docs/telegram-usage.md)

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

In this section you can change settings related to the WEB API so you can monitor from the browser dashboard utilizing [FreqUI](https://github.com/freqtrade/freqtrade/blob/develop/docs/rest-api.md)

The important settings to change are the **listen_port**, **jwt_secret_key**, and the **username** & **password** fields

If running multiple bots you want them to all point to the same CORS Origin so you only have to visit 1 browser address to manage all of them


## Docker Documentation & Guides

The following guides are from Freqtrade and can get you started with additional learning materials related to Docker & Freqtrade (To assist you with doing customizations)

[Quickstart](https://www.freqtrade.io/en/stable/docker_quickstart/)

[Rest API for Web](https://www.freqtrade.io/en/latest/rest-api/)

[Telegram](https://www.freqtrade.io/en/latest/telegram-usage/)