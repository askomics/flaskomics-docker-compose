# FlAskOmics - docker-compose files


A docker-compose file to deploy FlAskOmics with all its dependencies. It includes:

- flaskomics
- celery-flaskomics (flaskomics worker)
- redis (database, celery dependency)
- virtuoso (triplestore)
- nginx (web proxy)

## Install docker and docker-compose
```bash
# docker
sudo curl -sSL https://get.docker.com/ | sh
# docker-compose
# Debian/ubuntu
sudo apt install -y docker-compose
# Fedora
sudo dnf install -y docker-compose
```

## Run FlAskOmics

Clone this repository

```bash
git clone https://github.com/xgaia/flaskomics-docker-compose.git
```

Pull images

```bash
docker-compose pull
```

Run dockers

```bash
docker-compose up -d
```

See logs

```bash
docker-compose logs -f
```

Update

```bash
# Down, pull and re-run
docker-compose down
docker-compose pull
docker-compose up -d
```