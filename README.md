# FlAskOmics - docker-compose file

Some docker-compose files to deploy AskOmics and all its dependencies.

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

## [Standalone](standalone)

Deploy AskOmics to perform queries on your own data

- flaskomics
- celery-flaskomics (flaskomics worker)
- redis (database, celery dependency)
- virtuoso (triplestore)
- nginx (web proxy)
- (optional) ouroboros (auto-updater)

## [Federated](federated)

Deploy AskOmics with a federated query engine to perform queries on multiple endpoints

- flaskomics
- celery-flaskomics (flaskomics worker)
- redis (database, celery dependency)
- virtuoso (triplestore)
- corese (federated query engine)
- nginx (web proxy)
- (optional) ouroboros (auto-updater)


## Configure AskOmics


All properties defined in `askomics.ini` can be configured via the environment variables defined in the file `askomics.env`. The environment variable should be prefixed with `ASKO_` and have a format like `ASKO_$SECTION_$KEY`. `$SECTION` and `$KEY` are case sensitive.

If you use AskOmics in production mode, the following 3 properties have to be updated with random string:

- `ASKO_flask_secret_key`
- `ASKO_askomics_password_salt`
- `ASKO_triplestore_password`

You can easily obtain a random string with these commands:

```bash
python3 -c 'import random as r, string as s; print("".join(r.choices(s.printable[:62], k=20)))'
# or
pwgen -s 20 1
```

`ASKO_triplestore_password` have to be the same as `DBA_PASSWORD` in the virtuoso image.


## Configure Virtuoso

All properties defined in `virtuoso.ini` can be configured via the environment variables defined in `virtuoso.env`. The environment variable should be prefixed with `VIRT_` and have a format like `VIRT_$SECTION_$KEY`. `$SECTION` and `$KEY` are case sensitive.

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


## Auto update

**Ouroboros watch all dockers on your machine, don't use it on a shared environment**

Uncomment the ouroboros image if you want your AskOmics to be auto-updated when a new version is released.

## Run FlAskOmics

Clone this repository

```bash
# Clone
git clone https://github.com/xgaia/flaskomics-docker-compose.git
# cd
cd flaskomics-docker-compose
```

Update the config

```bash
vim askomics.env
vim virtuoso.env
```

Pull images

```bash
sudo docker-compose pull
```

Run dockers

```bash
sudo docker-compose up -d
```

See logs

```bash
sudo docker-compose logs -f
```

Update

```bash
# Down, pull and re-run
sudo docker-compose down
sudo docker-compose pull
sudo docker-compose up -d
```

Update with complete reset (database, files, rdf graphs ...)

```bash
# Down, pull and re-run
sudo docker-compose down
sudo rm -rf output
sudo docker-compose pull
sudo docker-compose up -d
```
