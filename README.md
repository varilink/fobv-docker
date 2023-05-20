# FoBV - Docker

David Williamson @ Varilink Computing Ltd

------

Docker setup for running The Friends of Bennerley Viaduct website locally, for development and testing purposes. Contains a set of services to simulate the website locally and tools to perform development actions on the website, all defined as Docker Compose services.

## Contents

| Location                                       | Contents                                                                                                               |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| tools/                                         | Varilink Computing Ltd tools that are used by this repository, included here as submodules.                            |
| wordpress/                                     | The [FoBV - WordPress](https://github.com/varilink/fobv-wordpress) repository, included here as a submodule.           |
| .env<br>docker&#8209;compose.yml<br>public.env | Project Docker Compose configuration, excluding secret environment variables that aren't tracked for security reasons. |

## Usage

Install project NPM dependencies:
```bash
docker-compose run --rm npm install
```

Generate Bootstrap CSS files in expanded style:
```bash
docker-compose run --rm sass /scss/custom.scss /css/bootstrap.css
```

Generate Bootstrap CSS files in compressed style:
```bash
docker-compose run --rm sass /scss/custom.scss /css/bootstrap.css --style compressed
```

Generate Bootstrap CSS files in compressed style, explicitly name as "min" files:
```bash
docker-compose run --rm sass /scss/custom.scss /css/bootstrap.min.css --style compressed
```

Bring up the website (brings up db service too as a dependency):
```bash
docker-compose up wordpress
```

Initialize the website
```bash
docker-compose run --rm wp-cli _bash /wp-scripts/init.sh
```

Restore from backup

Backup files in `/backup` folder.

Step 1:
```bash
docker-compose stop
docker-compose rm
docker volume rm fobv_wordpress
docker-compose up db
```

Allow some time for the db service to restore the database from backup then stop db service with `Ctrl+C`.

Step 2:
```bash
docker-compose up wordpress
```

Step 3 (separate terminal):
```bash
docker-compose run --rm wp-cli _bash /wp-scripts/restore.sh
docker-compose run --rm wp-cli _restore-media
```
