## How to run application:

### Prerequisites

* PostgreSQL installed and database created
* substitution of all environment variables (see `config.env.example` file)

#### Authentication

To use mocked ldap authentication:

* the variable `ldap.authentication.enabled` has to be set to `false` (see `config.env.example` file)
* mocked ldap data are in the file `ldap-mock-data.ldif` in core project

#### Coordinator user

Coordinator user has to be added manually to the database, to the `USER_DATA` table, and has to have the
COORDINATOR role assigned (table `USER_ROLES`)
The coordinator user index number corresponds to the LDAP username.


## How to run application using Docker:

### Prerequisites

#### Secrets

See `config.env.example` file.

#### Docker compose

Two docker compose files are available: `docker-compose-no-ssl.yml` and `docker-compose-ssl.yml`.
For development purpose use `docker-compose-no-ssl.yml` profile as it does not require SSL certificates.
`docker-compose-ssl.yml` is dedicated for production where connection should be secured.

#### Profiles

Both docker composes use different Spring profiles.
Since `docker-compose-no-ssl.yml` is used to build the development environment, 
it uses `application-docker-dev.properties` from the `core` project. \
On the other hand, `docker-compose-ssl.yml` uses `application-docker-prod.properties` for production configuration.

#### Certificates

Place the SSL certificates in the `/etc/ssl` directory. The volume for certificates is created in the `docker-compose-ssl.yml` file.


### Starting the application

To run the application and rebuild Docker images use command:

```
docker compose -f docker-compose-${ssl}.yml --env-file ${path} up --build
```

To restart the application without Docker images rebuild use command:

```
docker compose -f docker-compose-${ssl}.yml --env-file ${path} up`
```

`${ssl}` possible values: `no-ssl`,`ssl`. \
`${path}` path to the config.env file (e.g. `--env-file config.env`)
