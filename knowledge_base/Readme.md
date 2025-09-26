# Docker
Examples of docker compose : https://github.com/docker/awesome-compose

# Cli commands
## Dump stargate-postgres database

```bash
docker exec -it "docker_container" pg_dump --username="postgres" --host="localhost" --port=5432 --dbname "database_name" --format=plain --data-only --inserts --file "/home/db_dump.sql"
```

## Keycloak
Export all realms
```bash
docker exec -u root -it "docker_container" /opt/keycloak/bin/kc.sh export --dir /opt/keycloak/data/import --users realm_file
```

Export one realm
```bash
docker exec -u -it "docker_container" /opt/keycloak/bin/kc.sh export --dir /opt/keycloak/data/import --realm "realm_name" --users realm_file
```

Docker-compose to dump keycloak data.
Don't forget to stop container that run keycloak before running this one.
```yaml
services:
  keycloak: # auth server
    image: quay.io/keycloak/keycloak:${KEYCLOAK_VERSION}
    command: export --dir /opt/keycloak/data/import --users realm_file --optimized
    ports:
      - "${KEYCLOAK_PORT}:8080"
    volumes:
      # data to import
      - "./volume/data-import/:/opt/keycloak/data/import"
      # Add database as a volume, if not present only the master realm is imported
      - "./volume/database/:/opt/keycloak/data/h2"
      - "./volume/log/:/opt/keycloak/data/log"
    environment:
      KEYCLOAK_ADMIN: "${KEYCLOAK_ADMIN}"
      KEYCLOAK_ADMIN_PASSWORD: "${KEYCLOAK_ADMIN_PASSWORD}"
```