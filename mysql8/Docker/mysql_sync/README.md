# MySQL sync

## Copy private_key

```bash
cp ~/.ssh/your_key ./id_rsa
```

## Setting

### docker-compose.yml

```yaml
  mysql_sync:
    build: Docker/mysql_sync
    container_name: ${COMPOSE_PROJECT_NAME}_mysql_sync
    environment:
      TZ: Asia/Tokyo
      MYSQL_SYNC_REMOTE_HOST_NAME:              ${MYSQL_SYNC_REMOTE_HOST_NAME}
      MYSQL_SYNC_REMOTE_HOST_PORT:              ${MYSQL_SYNC_REMOTE_HOST_PORT}
      MYSQL_SYNC_REMOTE_HOST_USER_NAME:         ${MYSQL_SYNC_REMOTE_HOST_USER_NAME}
      MYSQL_SYNC_REMOTE_HOST_USER_PASSWORD:      #require when REMOTE_RSA_AUTH=false
      MYSQL_SYNC_REMOTE_HOST_USER_KEY_PATH:     ${MYSQL_SYNC_REMOTE_HOST_USER_KEY_PATH}
      MYSQL_SYNC_REMOTE_HOST_BACKUP_PATH:       ${MYSQL_SYNC_REMOTE_HOST_BACKUP_PATH}
      MYSQL_SYNC_REMOTE_HOST_RSA_AUTH:          ${MYSQL_SYNC_REMOTE_HOST_RSA_AUTH}
      MYSQL_SYNC_REMOTE_MYSQL_HOST:             ${MYSQL_SYNC_REMOTE_MYSQL_HOST}
      MYSQL_SYNC_REMOTE_MYSQL_PORT:             ${MYSQL_SYNC_REMOTE_MYSQL_PORT}
      MYSQL_SYNC_REMOTE_MYSQL_USER_NAME:        ${MYSQL_SYNC_REMOTE_MYSQL_USER_NAME}
      MYSQL_SYNC_REMOTE_MYSQL_USER_PASSWORD:    ${MYSQL_SYNC_REMOTE_MYSQL_USER_PASSWORD}
      MYSQL_SYNC_REMOTE_MYSQL_DATABASE_NAME:    ${MYSQL_SYNC_REMOTE_MYSQL_DATABASE_NAME}
      MYSQL_SYNC_REMOTE_DUMP_FILE_NAME_PREFIX:  ${MYSQL_SYNC_REMOTE_DUMP_FILE_NAME_PREFIX}
      MYSQL_SYNC_LOCAL_SAVE_PATH:               ${MYSQL_SYNC_LOCAL_SAVE_PATH}
      MYSQL_SYNC_MYSQL_SERVICE_NAME:            $MYSQL_SYNC_MYSQL_SERVICE_NAME
      MYSQL_USER:                               ${DB_USER}
      MYSQL_PASSWORD:                           ${DB_PASSWORD}
      MYSQL_DATABASE:                           ${DB_DATABASE}
    volumes:
      - ${DIR_PATH}/Docker/mysql_sync/dump:${MYSQL_SYNC_LOCAL_SAVE_PATH}:rw
      - ${DIR_PATH}/Docker/mysql_sync/scripts/sql_sync:/usr/local/bin/sync:rw
      - ${DIR_PATH}/Docker/mysql_sync/scripts/sql:/usr/local/bin/sql:rw
```

### env

```env
##############################
# mysql
##############################
COMPOSE_PROJECT_NAME=mysql_sync
DIR_PATH=.
DB_HOST=mysql
DB_SCHEMA=shop
DB_DATABASE=shop
DB_USER=<mysql_user>
DB_PASSWORD=<mysql_password>
DB_ROOT_PASSWORD=root
DB_EXTERNAL_PORT=3306
DB_INTERNAL_PORT=3306

##############################
# mysql_sync Remote host ssh
##############################
MYSQL_SYNC_REMOTE_HOST_NAME=<remote_host_IP_or_Domain>
MYSQL_SYNC_REMOTE_HOST_PORT=<remote_host_ssh_port>
MYSQL_SYNC_REMOTE_HOST_USER_NAME=<remote_host_login_user_name>
MYSQL_SYNC_REMOTE_HOST_USER_PASSWORD=  #require when REMOTE_RSA_AUTH=false, not working yet.
MYSQL_SYNC_REMOTE_HOST_USER_KEY_PATH=~<remote_host_login_user_private_key>
MYSQL_SYNC_REMOTE_HOST_BACKUP_PATH=/tmp
MYSQL_SYNC_REMOTE_HOST_RSA_AUTH=true

##############################
# mysql_sync Remote host MySQL
##############################
MYSQL_SYNC_REMOTE_MYSQL_HOST=<remote_mysql_host>
MYSQL_SYNC_REMOTE_MYSQL_PORT=<remote_mysql_port>
MYSQL_SYNC_REMOTE_MYSQL_USER_NAME=<remote_mysql_user>
MYSQL_SYNC_REMOTE_MYSQL_USER_PASSWORD=<remote_mysql_password>
MYSQL_SYNC_REMOTE_MYSQL_DATABASE_NAME=<remote_mysql_database_name>
MYSQL_SYNC_REMOTE_DUMP_FILE_NAME_PREFIX=dump_


##############################
# mysql_syncl local save path
##############################
MYSQL_SYNC_LOCAL_SAVE_PATH=/tmp/dump
MYSQL_SYNC_MYSQL_SERVICE_NAME=mysql # MySQL本体のs servicename

```

## Usage

```bash
# sync
docker-compose exec mysql_sync sync 
# latest restore ( Rollback )
docker-compose exec mysql_sync sync rb 
# ls dumpfile 
docker-compose exec mysql_sync sync ls
# remove dumpfile
docker-compose exec mysql_sync sync rm
```