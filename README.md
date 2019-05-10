# kubectl-run

## PostgreSQL (psql)

```sh
kubectl run psql --image=postgres:10 --rm -it --limits='cpu=0' -- psql -h $postgres_host -U $postgres_user -W postgres
```
