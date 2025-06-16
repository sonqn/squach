# Introduction
When you need to connect to some service as if you were in the same network as the Kubernetes cluster, you can use a Pod with socat to create a tunnel to the service.
# How to do

- socat will be running in a pod in kubernetes
- socat will essentially just be ‘forwarding’ postgres’s default port
- We’ll need to use kubectl to forward a local port to the socat pod
1. Way 1 

First you'll have to create the Pod, instructing socat to listen on a port and forward the traffic to the service you want to connect to. For example, to forward traffic from port 3306 to a MySQL service, you can create a Pod with the socat.yaml like this:
```
apiVersion: v1
kind: Pod
metadata:
  name: socat-proxy
  labels:
    app: socat-proxy
spec:
  containers:
  - name: socat
    image: alpine/socat:latest
    args:
      - TCP-LISTEN: 5432,fork
      - TCP:<EXTERNAL_MYSQLSQL_HOST>:5432
    ports:
      - containerPort: 5432

```
Run command to deploy the pod to Kubernetes: 
```
kubectl create -f socat.yaml
```
Verify the Pod is running:
'''
kubectl get pods
```
Once the Pod is running, you'll have to start a port-forwarding to the Pod:
```
kubectl port-forward pod/socat-proxy 5432:5432
```
This is going to create a tunnel from your local machine to the Pod. Now you can connect to the MySQL service as if it were running on your local machine:
```
$ mysql --local-infile=1 -h 127.0.0.1 -u admin -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 263848
Server version: 8.0.23 Source distribution

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> load data local infile '/Users/jordiprats/tmp/demodata.csv' into table demo.demo(id);
Query OK, 3 rows affected (0.13 sec)
Records: 3  Deleted: 0  Skipped: 0  Warnings: 0

mysql> select * from demo.demo;
+------+
| id   |
+------+
|    1 |
|    2 |
|    3 |
+------+
3 rows in set (0.07 sec)

mysql> ^DBye
```
You also can use a client tool such as DBeaver to connect the DB as local.
Delete pod when done:
kubectl delete socat.yaml 

2. Way 2 (Docker)

![idea_state_ports](uploads/7605c2b7975c716b0983500deb886b58/idea_state_ports.png)

```
# This is the host address of where the database is running. This is stored
# in a secret location...
DB_HOST=postgresqlservery81fz836.postgres.database.azure.com
NAMESPACE=my_apps_namespace
SOCAT_POD_NAME=postgres-db-proxy

# Run socat in a pod in kubernetes
kubectl run -n ${NAMESPACE} --restart=Never --image=alpine/socat \
    ${SOCAT_POD_NAME} -- \
    tcp-listen:5432,fork,reuseaddr \
    tcp-connect:${DB_HOST}:5432

# Wait for the pod to be ready
kubectl wait -n ${NAMESPACE} --for=condition=Ready pod/${SOCAT_POD_NAME}

# Forward port 5432 to the pod
kubectl port-forward -n ${NAMESPACE} pod/${SOCAT_POD_NAME} 5432:5432

# The moment of truth ... will it connect?
docker run -it --rm postgres psql -h host.docker.internal -U my_user -d my_db
> Password for user postgres:
> psql (14.1 (Debian 14.1-1.pgdg110+1))
> Type "help" for help.
>
> postgres=#


# Delete the pod when I'm done
kubectl delete -n ${NAMESPACE} pod/${SOCAT_POD_NAME} --grace-period 1 --wait=false

```