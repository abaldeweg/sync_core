# core

Sync backend for apps.

## Requirements

- PHP 8.0
- MySQL
- SSH Access
- PHP Composer
- Symfony Binary (dev only)

## Getting Started

Clone the repository:

```shell
git clone https://github.com/abaldeweg/sync.git
```

Create the file `.env.local` and `.env.test.local` with the following content. Please fit it to your needs.

```shell
DATABASE_URL=mysql://DB_USER:DB_PASSWORD@localhost:3306/DB_NAME
```

Then install the composer dependencies and create the database.

In `prod` run the following command.

```shell
bin/setup
```

If you are in `dev` run the following commands.

```shell
composer install
bin/console doctrine:database:create
bin/console doctrine:migrations:migrate -n
bin/console doctrine:fixtures:load -n
```

The fixtures will create an account `admin` with password `password`.

## Auth

To authenticate your users, you need to generate the SSL keys under `config/jwt/`.

```shell
bin/console lexik:jwt:generate-keypair
```

## Apache Webserver

Point the web root to the `public` dir.

You also need this header for apache.

```apache
SetEnvIf Authorization "(.*)" HTTP_AUTHORIZATION=$1
```

More info on that: <https://github.com/lexik/LexikJWTAuthenticationBundle/blob/master/Resources/doc/index.md#important-note-for-apache-users>

Configure your webserver to redirect all requests to the `index.php` file.

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{ENV:REDIRECT_STATUS} ^$
  RewriteRule ^index\.php(?:/(.*)|$) %{ENV:BASE}/$1 [R=301,L]
  RewriteCond %{REQUEST_FILENAME} -f
  RewriteRule ^ - [L]
  RewriteRule ^ %{ENV:BASE}/index.php [L]
</IfModule>
```

## Update

Just call the following command, if you are updating the production environment.

```shell
bin/setup
```
