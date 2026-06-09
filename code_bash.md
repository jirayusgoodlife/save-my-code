## Docker compose down with docker id
```sh
CONTAINER_ID="$(docker ps -qf "name=CONTAINER_NAME")"
cd "$(docker inspect --format '{{ index .Config.Labels "com.docker.compose.project.working_dir" }}' $CONTAINER_ID)" && docker compose down
```