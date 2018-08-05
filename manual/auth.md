## Authentication and SSO

### Configure JWT Store

When configuring a user db, add the following

```
issueId='12213' jwtStore="jwt.db" secret="" aesKey=""
```

The issueId is a long that pre-allocated for each site.  The jwtStore will configure a JWT store to store all known JWT.  The secret and aesKey 
are associated with the given issueId.  One define them here only in the development phase.  In production, one must load the secret and aesKey
to the JWT store using the jwt tool.

During production, find the dir where jwt.db is.  Create a jwt.db.secret file under the same dir.  In this file, specify a secret and an
aesKey separated by ','.  The secret and aesKey are for the JWT store only.

### Creating a JWT store

1. Create a secret and aes key for the store 

```
dbtools  aes -secret mySecret
```

Change mySecret to whatever secret message you may have.  This should generate an aes key for you.

2. Create a JWT store and add an issue id

```
dbtools  jwt -storeSecret mySecret  -storeAes FFCVKQeJ7UQojIDKa9fxAkuFETKDm7yYofU4NVhcsu4= -issueId 1000 -store jwt.db  add -secret issueSecret
```

The storeSecret and storeAes are what we created from the first step.  We now specify an issueId 1000 with a secret 'issueSecret'.  This secret
is associated with the issueId only.  Don't confuse it with the store secret. 

You can run the above command multiple times to create more issueId.  If you use the same issueId, the old data will be overwritten.

If you want to load a known aes key, just use the '-aes' to specify an aes key:

```
dbtools  jwt -storeSecret mySecret  -storeAes FFCVKQeJ7UQojIDKa9fxAkuFETKDm7yYofU4NVhcsu4= -issueId 1000 -store jwt.db  add -secret issueSecret -aes jy0jlf1UpB0IOVMFFChXpDpj1MwjwLw233OsLhiUXwA=
```


If you want to find out the secret and aesKey for an issueId, just do

```
dbtools  jwt -storeSecret mySecret  -storeAes FFCVKQeJ7UQojIDKa9fxAkuFETKDm7yYofU4NVhcsu4= -issueId 1000 -store jwt.db  
```

