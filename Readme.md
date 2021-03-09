# Steps

1. Make sure you use Hasura CLI 1.3.3
```
hasura update-cli --version 1.3.3
```

2. Start the environment
```
docker-compose up -d
```

3. Run hasura console
```
cd project
hasura console --admin-secret secret
```

4. Create basic table in the public schema and grant select permissions to the anonymous user
5. Add operation to the Allow list
Operation name: getPublicUsers
```
query getPublicUsers {
  users {
    id
    name
  }
}
```
