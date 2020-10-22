# kubectl-snippets

Here are snippets of `kubectl`.


## Query

- https://kubernetes.io/docs/reference/kubectl/overview/#custom-columns

### Get pods with resources requests

```
% kubectl get po --all-namespaces -o "custom-columns=NAME:.metadata.name,IP:.status.hostIP,RES:.spec.containers[0].resources.requests" --sort-by .status.hostIP
NAME                                                                  IP              RES
kube-dns-6c4cb66dfb-w69l7                                             172.20.57.157   map[cpu:100m memory:70Mi]
heapster-heapster-697757c69d-4fjpr                                    172.20.57.157   map[cpu:190m memory:224Mi]
kube-apiserver-ip-172-20-60-136.us-west-2.compute.internal            172.20.60.136   map[cpu:150m]
kube-scheduler-ip-172-20-60-136.us-west-2.compute.internal            172.20.60.136   map[cpu:100m]
kube-controller-manager-ip-172-20-60-136.us-west-2.compute.internal   172.20.60.136   map[cpu:100m]
kube-proxy-ip-172-20-60-136.us-west-2.compute.internal                172.20.60.136   map[cpu:100m]
```

### Get nodes with labels and taints

```
% kubectl get nodes -ocustom-columns=NAME:.metadata.name,CREATED:.metadata.creationTimestamp,LABELS:.metadata.labels,TAINTS:.spec.taints
NAME                                               CREATED                LABELS                                                                                                                                                                                                                                                                                                                                                    TAINTS
ip-172-19-68-180.ap-northeast-1.compute.internal   2019-12-02T07:51:11Z   map[beta.kubernetes.io/arch:amd64 beta.kubernetes.io/instance-type:m4.large beta.kubernetes.io/os:linux failure-domain.beta.kubernetes.io/region:ap-northeast-1 failure-domain.beta.kubernetes.io/zone:ap-northeast-1a kubernetes.io/arch:amd64 kubernetes.io/hostname:ip-172-19-68-180.ap-northeast-1.compute.internal kubernetes.io/os:linux]           <none>
ip-172-19-70-235.ap-northeast-1.compute.internal   2019-12-02T09:05:53Z   map[app:foo beta.kubernetes.io/arch:amd64 beta.kubernetes.io/instance-type:m4.large beta.kubernetes.io/os:linux failure-domain.beta.kubernetes.io/region:ap-northeast-1 failure-domain.beta.kubernetes.io/zone:ap-northeast-1a kubernetes.io/arch:amd64 kubernetes.io/hostname:ip-172-19-70-235.ap-northeast-1.compute.internal kubernetes.io/os:linux]   [map[effect:NoExecute key:app value:foo]]
ip-172-19-71-23.ap-northeast-1.compute.internal    2019-12-02T09:05:44Z   map[app:bar beta.kubernetes.io/arch:amd64 beta.kubernetes.io/instance-type:m4.large beta.kubernetes.io/os:linux failure-domain.beta.kubernetes.io/region:ap-northeast-1 failure-domain.beta.kubernetes.io/zone:ap-northeast-1a kubernetes.io/arch:amd64 kubernetes.io/hostname:ip-172-19-71-23.ap-northeast-1.compute.internal kubernetes.io/os:linux]    [map[effect:NoExecute key:app value:bar]]
```

## Run

### PostgreSQL (psql)

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

#### Enable pg_tram

You can enable pg_tram for a specific database as:

```
% kubectl run psql --image=postgres:10 --rm -it -- psql -h HOST -U USER -W DATABASE
```

```sql
CREATE EXTENSION pg_trgm;
```

### AWS CLI

To enter the shell:

```console
% kubectl run aws -it --rm --image amazon/aws-cli --restart=Never --command -- /bin/bash
bash-4.2# aws --debug --region ap-northeast-1 s3 ls
```

To list S3 buckets:

```sh
kubectl run s3 -i --rm --image amazon/aws-cli --restart=Never -- s3 ls

# IRSA
kubectl create sa s3-test
kubectl annotate sa s3-test "eks.amazonaws.com/role-arn=arn:aws:iam::ACCOUNT:role/NAME"
kubectl run s3 -i --rm --image amazon/aws-cli --restart=Never --serviceaccount=s3-test -- s3 ls

# kube2iam
kubectl run s3 -i --rm --image amazon/aws-cli --restart=Never \
  --overrides '{"metadata":{"annotations":{"iam.amazonaws.com/role":"ROLE"}}}' -- s3 ls
```
