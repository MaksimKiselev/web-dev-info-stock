# DOCKER


### PostgreSQL DB container dump

Backup your databases
```bash
docker exec -t YOUR_DB_CONTAINER_NAME pg_dumpall -c -U postgres > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql
```

Restore your databases
```bash
cat YOUR_DUMP.sql | docker exec -i YOUR_DB_CONTAINER_NAME psql -U postgres
```

### Quick clear all docker logs
```bash
truncate -s 0 /var/lib/docker/containers/*/*-json.log
```
