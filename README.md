## Deprecated

This image is deprecated and the focus of development will be on the [Docker Library Official Image](https://hub.docker.com/_/mariadb) (source: [mariadb-docker](https://github.com/MariaDB/mariadb-docker)).

Please report any blocking incompatibilities that affect your transition on [JIRA](https://jira.mariadb.org/).

## Maintained by: [MariaDB](https://mariadb.com/)

This is the Git repo of docker images for MariaDB Server, [mariadb-server-docker](https://github.com/mariadb-corporation/mariadb-server-docker)

---

-	[Travis CI:  
	![build status badge](https://img.shields.io/travis/mariadb-corporation/mariadb-server-docker/master.svg)](https://travis-ci.org/mariadb-corporation/mariadb-server-docker/branches)

---

### Installation
Include the the version number as a docker tag.

To pull MariaDB Server version 10.3 run the following command:
```
docker pull mariadb/server:10.3
```

To run MariaDB Server version 10.3 run the following command:
```
docker run -d --name maria -eMARIADB_ROOT_PASSWORD=mypassword mariadb/server:10.3
```

### Configuration
The following environment variables can be utilized to configure behavior (one of the first three must be specified):
* MARIADB_ROOT_PASSWORD : specify the password for the root user
* MARIADB_ALLOW_EMPTY_PASSWORD : allow empty password for the root user
* MARIADB_RANDOM_ROOT_PASSWORD : generate a random password for the root user (output to logs)
* MARIADB_INITDB_SKIP_TZINFO : skip timezone setup
* MARIADB_ROOT_HOST : host for root user, defaults to '%'
* MARIADB_DATABASE : create a database with this name
* MARIADB_USER : create a user with this name, with all privileges on MARIADB_DATABASE if specified
* MARIADB_PASSWORD : password for above user

For backward compatibility, environment variables starting MYSQL_ rather than MARIADB_ will also work.

Custom scripts can be mapped as a host volume to /docker-entrypoint-initdb.d for execution post initialization in named / directory order. The following extensions are supported:
* .sh : shell script
* .sql : sql script
* .sql.gz : gzip compressed sql script
