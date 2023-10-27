# FoBV - Docker

David Williamson @ Varilink Computing Ltd

------

Docker setup for running The Friends of Bennerley Viaduct website locally, for development and testing purposes. Contains a set of services to simulate the website locally and tools to perform development actions on the website, all defined as Docker Compose services.

## Contents

| Location                                                    | Contents                                                                                                               |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| tools/                                                      | Varilink Computing Ltd tools that are used by this repository, included here as submodules.                            |
| wordpress/                                                  | The [FoBV - WordPress](https://github.com/varilink/fobv-wordpress) repository, included here as a submodule.           |
| .env<br>docker&#8209;compose.yml<br>dummy.yml<br>public.env | Project Docker Compose configuration, excluding secret environment variables that aren't tracked for security reasons. |

## Usage

### Install Project's Node Dependencies

WordPress theme:
```sh
docker-compose run --rm --volume="$PWD/wordpress/theme/:/npm/" npm install
```

Plugins (only if scaffolding new blocks using @wordpress/create-block):
```sh
docker-compose run --rm --volume="$PWD/wordpress/plugins/:/npm/" npm install
```

FoBV event plugin:
```sh
docker-compose run --rm --volume="$PWD/wordpress/plugins/fobv-event/:/npm/" npm install
```

### Build Project's Client Assets

Theme additional CSS:
```sh
docker-compose run --rm --volume="$PWD/wordpress/theme/:/npm/ npx sass --style=compressed --no-source-map for-theme-json.scss for-theme-json.css
```
Note: The output, which is written to `for-theme-json.css` must then be copied to `theme.json` as the value of `styles->css` there.

Theme images (generated using GIMP):
```sh
docker-compose run --rm gimp
```
Note: First ensure that the directory `wordpress/media/` is created.

Build FoBV event plugin's javascript and stylesheet assets:
```sh
docker-compose run --rm --volume="$PWD/wordpress/plugins/fobv-event/:/npm/" npm run build
```
