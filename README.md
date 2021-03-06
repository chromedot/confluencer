# Atlassian Confluence
Docker Compose setup for Atlassian Confluence. This sets up two docker containers: one hosting Confluence with proper Oracle Java JDK, and one with a PostgreSQL database.

* Based on: https://hub.docker.com/r/atlassian/confluence-server
* Dockerfile repository: https://bitbucket.com/atlassian/docker-atlassian-confluence-server
* Current version: 7.5.0 / works with PostgreSQL 12

## Installation

* Update `settings.env` and `secrets.env`
* make sure `startup.sh` and `shutdown.sh` are executable
* delete all folders in `data`
* Create log folder, change owner `cd data && mkdir logs && sudo chown daemon:daemon logs`

```bash
# to create/start the Confluence and Postgres containers
./startup.sh

# to shut down and remove the Confluence and Postgres containers
./shutdown.sh
```

### Setup
Database settings are now stored in .env and no manual setup should be necessary.

#### Manual Setup
* Navigate to localhost:8090
* Select "Production" installation
* Select external PostgreSQL database
* Enter database connection string: `jdbc:postgresql://db:5432/confluence`
* Enter database user name `confluence` and password

### Installed Plugins
* Draw.io (paid license)
* Markdown macro for Confluence (free)
* No email storm (free)

#### Migrating from Confluence Cloud to Server

 * [Migrate from Cloud to Server](https://confluence.atlassian.com/confcloud/migrate-from-confluence-cloud-to-server-724765578.html)
 * [Restore password to recover admin user rights](https://confluence.atlassian.com/doc/restore-passwords-to-recover-admin-user-rights-158390.html)

```bash
docker-compose stop confluence-server
docker run -ti -v $PWD/data/server:/var/atlassian/application-data/confluence -p 8090:8090 confluence/server bash
cd $CONFLUENCE_INSTALL_DIR/bin
vi setenv.sh

# add line
CATALINA_OPTS="-Datlassian.recovery.password=<password> ${CATALINA_OPTS}"

# exit, restart, log in with user: recovery_admin
./startup
user: recovery_admin
```

#### Getting access to Confluence after restoring a backup

#### Confluence Docker Image
 * Based on [Atlassian Confluence Server Docker Image](https://bitbucket.org/atlassian/docker-atlassian-confluence-server/overview)
 * Installs Confluence using Oracle Java JDK (details see [this article](https://confluence.atlassian.com/confkb/update-the-confluence-docker-image-to-use-oracle-jdk-829062521.html))
 * Confluence version: [6.6.7](http://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-6.6.7.tar.gz)
 
#### PostgreSQL Database Docker Image
 * Based on [Latest PostgreSQL Image](https://hub.docker.com/_/postgres/)