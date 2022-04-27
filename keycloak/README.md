# Keycloak with Docker behind Nginx with Certbot

### Setup:

Checkout the [main README](/README.md) for a Guide to install Docker and Docker-Compose.

Create a new Network for the Services and a Volume each for PostgreSQL and Certbot-Secrets.

```bash
sudo docker network create my-net
sudo docker volume create psql_storage
sudo docker volume create nginx_secrets
```

The files in the directory have some placeholder values that you need to change before you can run the application, they are:

 - `SOMETHING_LONG_AND_SECURE` as the KEYCLOAK_PASSWORD in `docker-compose.yml`
 - `YOUR_EMAIL` as the CERTBOT_EMAIL in `docker-compose.yml`
 - `YOUR_DOMAIN` as both `server_name`s in the two server-blocks and twice in the `ssl_certificate` and `ssl_certificate_key` values of the `user_conf.d/webserver_nginx.conf`

After the configuration is done, make sure that `YOUR_DOMAIN` is pointing to your server. You can check this by running

```bash
curl icanhazip.com && dig +short YOUR_DOMAIN
```

Which should print the servers ip-adress twice.

Then simply run the services:

```bash 
sudo docker-compose up -d
```
