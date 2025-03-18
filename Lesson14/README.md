# Kubernetes StatefulSet Example
An example of a Kubernetes StatefulSet

## Mounting persistent volumes on minikube

* `minikube mount www1:/tmp/www1 &`
* `minikube mount www2:/tmp/www2 &`

## Creating persistent volumes
* `kubectl apply -f www-volume1.yaml`
* `kubectl apply -f www-volume2.yaml`

## Deploying a StatefulSet
`kubectl apply -f statefulset.yaml`

## Deploy a sleep container for testing
`kubectl apply -f https://raw.githubusercontent.com/istio/istio/master/samples/sleep/sleep.yaml`

## Send Requests to the both nginx services
$ `kubectl exec -it $(kubectl get pods -o jsonpath='{.items[*].metadata.name}' -l app=sleep) -c sleep -- curl web-0.nginx`

Hello from Kubernetes storage 1

$ `kubectl exec -it $(kubectl get pods -o jsonpath='{.items[*].metadata.name}' -l app=sleep) -c sleep -- curl web-1.nginx`

Hello from Kubernetes storage 2

## Additional queries
* Check pods
```
$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
sleep-6bc775c9c5-9hrdn   1/1       Running   0          6h
web-0                    1/1       Running   0          2m
web-1                    1/1       Running   0          2m
```
* Check stateful sets
```
$ kubectl get statefulset
NAME      DESIRED   CURRENT   AGE
web       2         2         4m

```
* Check persistent volumes
```
$ kubectl get persistentvolume
NAME          CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM               STORAGECLASS       REASON    AGE
www-volume1   10Mi       RWO           Retain          Bound     default/www-web-0   my-storage-class             41s
www-volume2   10Mi       RWO           Retain          Bound     default/www-web-1   my-storage-class             34s
```
* Check persistent volume claims
```
$ kubectl get persistentvolumeclaim
NAME        STATUS    VOLUME        CAPACITY   ACCESSMODES   STORAGECLASS       AGE
www-web-0   Bound     www-volume1   10Mi       RWO           my-storage-class   2m
www-web-1   Bound     www-volume2   10Mi       RWO           my-storage-class   2m

```
