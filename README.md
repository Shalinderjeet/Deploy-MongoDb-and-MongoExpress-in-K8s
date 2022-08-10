# Deploy MongoDB and MongoExpress into localK8s(Minikube).

Description:- Setup local K8s cluster with Minikube
Deploy MongoDB and MongoExpressconfiguration and credentials extracted into ConfigMap and Secret.

Details:-

Create a basic template of a deployment of mongo database with a mongo image listening on port 27017.
Create a file name mongo-secret.yaml with a secret type Opaque.
Use $echo -n 'username' | base64 and $echo -n 'password' | base64 values in mongo-secret.yaml file
Run $kubectl apply -f mongo-secret.yaml

 $kubectl apply -f mongo-secret.yaml
 secret/mongodb-secret created

Refer the created secret values in mongo-secret.yaml deployment/pod file.

Create an Internal service for Mongodb deployment so that the other components can talk to this Mongodb.
Deployment and Mongodb service -put their configuration in the same file.

$kubectl apply -f mongo.yaml
deployment.apps/mongodb-deployment created
service/mongodb-service created

Create mongo-configmap.yaml file for Configmap holding the mongodb URL pointing to mongo-service (Internal)

$kubectl create -f mongo-configmap.yaml
configmap/mongodb-configmap created

Now, create mongo-express.yaml file for Mongo-express deployment/pod listening on port 8081
Also, create mongo-express service in the same yaml file

$kubectl apply -f mongo-express.yaml
deployment.apps/mongo-express created
service/mongo-express-service created

$kubectl logs mongo-express-98c6ff4b4-vss82
Welcome to mongo-express
------------------------


(node:7) [MONGODB DRIVER] Warning: Current Server Discovery and Monitoring engine is deprecated, and will be removed in a future version. To use the new Server Discover and Monitoring engine, pass option { useUnifiedTopology: true } to the MongoClient constructor.
Mongo Express server listening at http://0.0.0.0:8081

$kubectl get service
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP          9d
mongo-express-service   LoadBalancer   10.110.54.150   <pending>     8081:30000/TCP   11m
mongodb-service         ClusterIP      10.101.45.248   <none>        27017/TCP        21m


$minikube service mongo-express-service  
|-----------|-----------------------|-------------|---------------------------|
| NAMESPACE |         NAME          | TARGET PORT |            URL            |
|-----------|-----------------------|-------------|---------------------------|
| default   | mongo-express-service |        8081 | http://192.168.49.2:30000 |
|-----------|-----------------------|-------------|---------------------------|
🏃  Starting tunnel for service mongo-express-service.
|-----------|-----------------------|-------------|------------------------|
| NAMESPACE |         NAME          | TARGET PORT |          URL           |
|-----------|-----------------------|-------------|------------------------|
| default   | mongo-express-service |             | http://127.0.0.1:58793 |
|-----------|-----------------------|-------------|------------------------|
🎉  Opening service default/mongo-express-service in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it
This will open up your mongoexpress UI application in your browser.
