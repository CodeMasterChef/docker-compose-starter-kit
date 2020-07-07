# POSTGRESQL

Change `ports` if you want to map another port, example 4000:

```
 ports:
      - 4000:5432
```

Change `environment` to change username and password for database server:

```
POSTGRES_USER: dbadmin
POSTGRES_PASSWORD: AdminPassword
```

Change `container_name` to change the Docker container name:

```
container_name: my_postgres
```
