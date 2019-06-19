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

## Configure AskOmics

In `docker-compose.yml`, update the following values:

- askomics and celery_askomics images

    - ASKO_flask_secret_key: random string
    - ASKO_askomics_footer_message: custom message for your instance (optional)
    - ASKO_askomics_password_salt: random string
    - ASKO_triplestore_password: secure password for the triplestore, same password in the virtuoso image

## Configure Virtuoso

Update the `VIRT_Parameters_NumberOfBuffers` and `VIRT_Parameters_MaxDirtyBuffers` environments according to how much memory do you want to allow to Virtuoso:


| Memory available (GB) | NumberOfBuffers | MaxDirtyBuffers |
|-----------------------|-----------------|-----------------|
| 2                     | 170000          | 130000          |
| 4                     | 340000          | 250000          |
| 6                     | 510000          | 375000          |
| 8                     | 680000          | 500000          |
| 16                    | 1360000         | 1000000         |
| 32                    | 2720000         | 2000000         |
| 48                    | 4000000         | 3000000         |
| 64                    | 5450000         | 4000000         |


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

Update with complete reset (database, files, rdf graphs ...)

```bash
# Down, pull and re-run
docker-compose down
rm -rf output
docker-compose pull
docker-compose up -d
```
