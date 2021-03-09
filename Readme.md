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

## Update to Hasura 2.0
1. Update server image to v2.0.0-alpha.3 and update services
```
docker-compose up -d
```
2. Update CLI
```
hasura update-cli --version v2.0.0-alpha.3
```
3. Update project v2 -> v3
```
hasura scripts update-project-v3 --project project --admin-secret secret

WARN The upgrade process will make some changes to your project directory, It is advised to create a backup project directory before continuing
WARN This script will rewrite the project metadata with metadata on server
continue? (y/n): y
what database does the current migrations / seeds belong to?: default
INFO plugin installed                              name=cli-ext
```

4. Start the console
```
hasura console --project project --admin-secret secret
```

5. Create new table
Same structure as before, just call it `members` and grant permissions to the anonymous user.

6. Go to the `Settings -> Allow List`
Operation name: getPublicMembers
Operation:
```
query getPublicMembers {
  members {
    id
    name
  }
}
```

You will get an error:
```
{
    "path": "$.args[1].args[0].args",
    "error": "query with name \"getPublisMembers\" already exists in collection \"allowed-queries\"",
    "code": "already-exists"
}
```

If you try to query the API with this operation, you'll get:
```
{
  "errors": [
    {
      "extensions": {
        "internal": {
          "message": "query is not in any of the allowlists"
        },
        "path": "$",
        "code": "validation-failed"
      },
      "message": "query is not allowed"
    }
  ]
}
```
