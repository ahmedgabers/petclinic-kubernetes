# petclinic on K8s/MySQL

1. Setup an env varible to target the images:
```
export REPOSITORY_PREFIX=ahmedgabercod
```

2. Create the *spring-petclinic* namespace:
```
kubectl apply -f k8s/init-namespace/
```

3. Create the Kubernetes services that will be used by our deployments:
```
kubectl apply -f k8s/init-services
```

4. Setup the MySQL backend store using Helm:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install vets-db-mysql bitnami/mysql --namespace spring-petclinic --version 6.14.3 --set db.name=service_instance_db
helm install visits-db-mysql bitnami/mysql --namespace spring-petclinic  --version 6.14.3 --set db.name=service_instance_db
helm install customers-db-mysql bitnami/mysql --namespace spring-petclinic  --version 6.14.3 --set db.name=service_instance_db
```

5. Our deployment YAMLs have a placeholder called *REPOSITORY_PREFIX* so we'll be able to deploy the images from Docker Hub:
```
./scripts/deployToKubernetes.sh
```

6. Verify the pods are deployed:
```
kubectl get pods -n spring-petclinic
```

7. Get the IP address of the node and access the gateway using *$NODE_IP:32000* from a web browser:
```
echo $(kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}':32000)
```
