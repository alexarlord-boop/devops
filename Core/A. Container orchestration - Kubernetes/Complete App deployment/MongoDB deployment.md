# MongoDB deployment

* MongoDB is a popular NoSQL database that is used in many applications. 
* Mongo Express is a web-based admin interface for MongoDB that allows you to interact with your database through a web browser.


## Artifacts to be created
1. MongoDB Deployment
2. Mongo Express Deployment
3. MongoDB Service (internal)
4. Mongo Express Service (external)
5. MongoDB secret for DB credentials
6. MongoDB ConfigMap for DB configuration

## Request flow
1. Browser GUI of Mongo Express
2. Mongo Express external service
3. Mongo Express pod
4. MongoDB internal service (DB url from ConfigMap)
5. MongoDB pod (auth request with secret)

## Creation order
Referenced artifacts should be created first.
1. Secret
2. MongoDB Deployment
3. MongoDB Internal Service (in the same deployment file, because they are tightly coupled)
4. ConfigMap
5. Mongo Express Deployment
6. Mongo Express External Service


## Run

On macos, simply running ```minikube service mongo-express-service``` won't work, since we started minikube with the default [Docker driver](https://minikube.sigs.k8s.io/docs/drivers/).


You can try the alternative:
```bash
kubectl port-forward service/mongo-express-service 8080:8081
```

To authorize in Mongo Express, use the following credentials:
```
username: admin
password: pass
```