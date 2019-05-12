# kubectl-run

## PostgreSQL (psql)

```
% kubectl run psql --image=postgres:10 --rm -it -- psql -h $postgres_host -U $postgres_user -W postgres
If you don't see a command prompt, try pressing enter.

psql (10.8 (Debian 10.8-1.pgdg90+1), server 10.6)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=> 
```

You need to enter your password. After authentication you can execute SQLs, for example,

```sql
CREATE DATABASE keycloak;
CREATE USER keycloak PASSWORD 'keycloak';
GRANT ALL PRIVILEGES ON DATABASE keycloak TO keycloak;
```
