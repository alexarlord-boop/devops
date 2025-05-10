view:
	    nohup minikube dashboard  > minikube_dashboard.log 2>&1 & 

enable-cert-manager:
	    kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml

enable-ingress-nginx:
	    minikube addons enable ingress

init: enable-cert-manager enable-ingress-nginx


cluster:
	kubectl apply -f cluster-issuer.yaml
	kubectl apply -f certificate.yaml 
	kubectl apply -f dashboard-ingress.yaml 

clean:
	kubectl delete -f cluster-issuer.yaml
	kubectl delete -f certificate.yaml 
	kubectl delete -f dashboard-ingress.yaml

cluster-secret:
	kubectl apply -f tls-secret.yaml
	kubectl apply -f dashboard-ingress.yaml

cluster-secret-clean:
	kubectl delete -f tls-secret.yaml
	kubectl delete -f dashboard-ingress.yaml