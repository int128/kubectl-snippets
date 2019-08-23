# kubectl-run

Here are snippets of `kubectl run`.

## PostgreSQL (psql)

You can access to a host as:

```
% kubectl run psql --image=postgres:10 --rm -it -- psql -h HOST -U USER -W postgres
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

### Enable pg_tram

You can enable pg_tram for a specific database as:

```
% kubectl run psql --image=postgres:10 --rm -it -- psql -h HOST -U USER -W DATABASE
```

```sql
CREATE EXTENSION pg_trgm;
```

## AWS

To list S3 buckets:

```sh
kubectl run aws -i --rm --image fstab/aws-cli --restart=Never -- /home/aws/aws/env/bin/aws s3 ls

# kube2iam
kubectl run s3 -i --rm --image fstab/aws-cli --restart=Never --overrides '{"metadata":{"annotations":{"iam.amazonaws.com/role":"ROLE"}}}' -- /home/aws/aws/env/bin/aws s3 ls
```
