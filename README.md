# Dockerfiles and docker-compose.ymls for Nibyou's Services 

This repository includes the docker configuration files used to run our services. Some services also have their own `Dockerfile`s, which will not be copied to this repo but live in the respective repos of the services themselves.

To get started, here is a small tutorial on how to install docker and docker-compose on Ubuntu 20.04 LTS, which our servers currently run on.

## Docker
<sup>For possibly more up-to-date information, visit https://docs.docker.com/engine/install/ubuntu/ </sup>

To start off, remove all old versions of the docker suite:

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Update the apt-cache and install the needed software to add the sources list for dockers repo:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
```

Add dockers GPG Key and the the sources list to your apt sources:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Finally, update the apt-cache once more and install `docker-ce`, the `docker-ce-cli` and `containerd.io`:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Now, running `sudo docker --version` should not get you any errors.

## docker compose
<sup>For possibly more up-to-date information, visit https://docs.docker.com/compose/install/ </sup>

Docker-compose is now just a plugin for docker, you can install it using the `docker-compose-plugin` package:

```bash
sudo apt-get install docker-compose-plugin
```

To test the installation, you can run `sudo docker compose version`

With this done, check out the service's directories for Dockerfiles and docker-compose.ymls to run the services with.

## Logging to Loki
Instead of using `docker logs <container>` every time, Docker can be set up to pipe it's logs to a Loki Instace.

To enable this, first install the `grafana/loki-docker-driver` Docker plugin:

```bash
docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
```

And then, edit the Docker daemon config at `/etc/docker/daemon.json`:

```bash
sudo nano /etc/docker/daemon.json
```

And add the following configuration, with changes for the Loki server, of course:

```json
{
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "https://<user>:<password>@loki.example.com/loki/api/v1/push",
        "loki-batch-size": "400"
    }
}
```

Finally, restart Docker:

```bash
sudo systemctl restart docker
```
