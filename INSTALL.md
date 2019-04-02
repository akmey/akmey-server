# Setup instructions

## Requirements

- A server with:
    - Node.js runtime (10.x recommended) and npm
    - PHP with:
        - Internationalization extension
        - PDO extension (with a DB driver)
        - Mbstring extension
        - Tokenizer extension
        - XML extension
        - Ctype extension
        - JSON extension
        - BCMath extension
        - _Some of these deps are bundled into php packages, you can check that by generating a phpinfo(); page._
    - A webserver (apache2/nginx)
    - Go runtime (1.12.x)
    - Composer
    - A DB server (MariaDB, MySQL, Postgre, any other PDO/Laravel-compatible server)

## Part 1 - server-php

_Make sure the git submodules are downloaded, if not, do `git submodule init`._

server-php contains the webapp part, it's made with Laravel.

First, download dependencies via Composer and npm:

```Bash
composer install --no-dev
npm i
```

Next, fill your .env file:

```Bash
cp .env.example .env
nano .env
```

| Name                 | Description                                                                                                            |
|----------------------|------------------------------------------------------------------------------------------------------------------------|
| APP_ENV              | production, local                                                                                                      |
| APP_KEY              | It's generated automatically later, leave it                                                                           |
| APP_DEBUG            | false = production / true = debugging                                                                                  |
| APP_URL              | Akmey webapp endpoint                                                                                                  |
| SSH_ACCESS           | Akmey SSH server endpoint                                                                                              |
| DB_*                 | Database options, please see [Laravel documentation](https://laravel.com/docs/5.8/database#configuration) for details. |
| MAIL_*               | Mail options, please see [Laravel documentation](https://laravel.com/docs/5.8/mail#introduction) for details.          |
| GITHUB_CLIENT_ID     | GitHub OAuth app client ID, apps can be created [here](https://github.com/settings/developers).                        |
| GITHUB_CLIENT_SECRET | GitHub OAuth app client secret, apps can be created [here](https://github.com/settings/developers).                    |
| LEGAL_NAME           | Your name, as host.                                                                                                    |
| LEGAL_HOST           | Your host, such as AWS, Hetzner, OVH, etc...                                                                           |
| LEGAL_DETAILS        | Host details, such as address, website.                                                                                |
| LEGAL_MORE           | Custom text for /legal page                                                                                            |

_Note: GitHub callback URL is http://your.app.url/github/callback_

Now, generate app keys :

```Bash
php artisan key:generate
php artisan passport:install
```

Create DB schemas :

```Bash
php artisan migrate
```

Build client :

```Bash
cd resources/semantic
npx gulp build
cd ../..
npm run production
```

See [Laravel documentation](https://laravel.com/docs/5.8/installation#web-server-configuration) for webserver setup.

## Part 2 - server-go

Install UI deps :

```Bash
cd ui
npm i
```

Build :

```Bash
go build
```

Copy the configuration and edit it :

```Bash
cp config.ini.example config.ini
nano config.ini
```

Launch it via a systemd unit, or other system.

## Nice!

Should be good now! If you encounter issues, [contact me @ Telegram](https://t.me/leonekmi).