# Kubernetes


Run the Mongodb database container with MongoExpress in Kubernetes minikube cluster

Create a basic template of a deployment of mongo database with a mongo image listening on port 27017.
Create a file name mongo-secret.yaml with a secret type Opaque.
Use $echo -n 'username' | base64 and $echo -n 'password' | base64 values in mongo-secret.yaml file
Run $kubectl apply -f mongo-secret.yaml
Refer the created secret values in mongo-secret.yaml deployment/pod file.

Create an Internal service for Mongodb deployment so that the other components can talk to this Mongodb.
Deployment and Mongodb service -put their configuration in the same file.

