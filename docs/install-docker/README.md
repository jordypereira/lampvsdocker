# Install Docker

- Our server needs the following packages to work:

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

- Get the GPG Key

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

- Add the stabe docker repository `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- `sudo apt update` to update our package database

- And finally we can `sudo apt install docker-ce`

- Check if it's running with `sudo systemctl status docker`

## Adding user to the docker group

- `sudo usermod -aG docker ${USER}` or replace the \${USER} with the username
- `su - ${USER}`
- Check your current user groups with `id -nG`

## Docker-compose

- `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
- `sudo chmod +x /usr/local/bin/docker-compose`
