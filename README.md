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

## docker-compose
<sup>For possibly more up-to-date information, visit https://docs.docker.com/compose/install/ </sup>

While docker wants you to use `docker compose`, it's not yet feature-complete and thus, both installations are needed. Thankfully, this is going to be a fast one:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Notice that this will install 1.92.2 which may or may not be the newest supported version of compose 1.

Change the permissions to make the docker-compose script executable:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Create a symbolic link from the installation location to `/usr/bin`:

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

With this done, check out the service's directories for Dockerfiles and docker-compose.ymls to run the services with.
