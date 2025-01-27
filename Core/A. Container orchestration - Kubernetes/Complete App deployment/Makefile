
cluster: view
	kubectl apply -f secret.yaml
	kubectl apply -f configmap.yaml
	kubectl apply -f mongodb.yaml
	kubectl apply -f mongoexpress.yaml


check:
	kubectl get all

pods:
	kubectl get pods

services:
	kubectl get services

clean:
	kubectl delete -f secret.yaml
	kubectl delete -f configmap.yaml
	kubectl delete -f mongodb.yaml
	kubectl delete -f mongoexpress.yaml

expose:
	kubectl port-forward service/mongo-express-service 8080:8081

view:
	    nohup minikube dashboard  > minikube_dashboard.log 2>&1 & 