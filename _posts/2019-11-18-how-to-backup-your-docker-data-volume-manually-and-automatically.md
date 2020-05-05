---
title: "How to backup your docker data volume manually and automatically"
description: ""
category: "Virtualization"
tags: [docker,backups]
---
{% include JB/setup %}

# Manually 

[https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes]()

list containers
```
sudo docker ps -a
CONTAINER ID        IMAGE                      COMMAND                  CREATED            STATUS                        PORTS                    NAMES
000b7ce4a3ec        owncloud/server:10.0       "/usr/bin/entrypoint…"   3 months ago        Exited (137) 44 minutes ago                            owncloud_owncloud_1
fcf65a7b3f53        webhippie/redis:latest     "/usr/bin/entrypoint…"   3 months ago        Exited (0) 44 minutes ago                              owncloud_redis_1
a3560ff218ea        webhippie/mariadb:latest   "/usr/bin/entrypoint…"   3 months ago        Exited (0) 44 minutes ago                              owncloud_db_1
```

stop containers 
```
sudo docker stop owncloud_owncloud_1
```

backup data volumes 
```
sudo docker run --rm --volumes-from owncloud_db_1 -v /mnt/hdbackup:/backup ubuntu tar cvf /backup/owncloud-mysql-backup.tar /var/lib/mysql
sudo docker run --rm --volumes-from owncloud_owncloud_1 -v /mnt/hdbackup:/backup ubuntu tar cvf /backup/owncloud-files-backup.tar /mnt/data
```

# Automatically

To backup automatically add [blacklabelops/volumerize](https://hub.docker.com/r/blacklabelops/volumerize/)

list volumes 
```
sudo docker volume ls
DRIVER              VOLUME NAME
local               owncloud_backup
local               owncloud_files
local               owncloud_mysql
local               owncloud_redis
```

vi owncloud/docker-compose.yml 
```
volumes:
  backup_cache:
    driver: local

backups:
    image: blacklabelops/volumerize
    restart: unless-stopped
    environment:
        - TZ=Asia/Taipei
        - VOLUMERIZE_SOURCE=/source
        - VOLUMERIZE_TARGET=file:///backup
        - VOLUMERIZE_CONTAINERS=owncloud_owncloud_1 owncloud_db_1
        - VOLUMERIZE_JOBBER_TIME=0 0 7 * * *
        - VOLUMERIZE_FULL_IF_OLDER_THAN=14D
    volumes:
        - /var/run/docker.sock:/var/run/docker.sock
        - files:/source/owncloud_files:ro
        - mysql:/source/owncloud_mysql:ro
        - /mnt/hdbackup/owncloud:/backup
        - backup_cache:/volumerize-cache
```

running 
```
sudo docker-compose up -d backups 
```

debugging 
```
sudo docker logs -f owncloud_backups_1 
```

testing 
```
sudo docker exec owncloud_backups_1 backups 
```
