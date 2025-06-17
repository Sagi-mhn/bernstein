# Projet T-DOP-603  

# Commandes de lancement :

(lancer docker avant)  
## Cluster KIND  
kind create cluster --config=kind-config.yaml --name=dop603
kubectl cluster-info --context kind-dop603

## Namespace kube-public (Si n'existe pas)  
kubectl create namespace kube-public  
  
## Cadvisor  
kubectl apply -f cadvisor.daemonset.yaml  
  
## PostgreSQL  
kubectl apply -f postgres.secret.yaml -f postgres.configmap.yaml -f postgres.volume.yaml -f postgres.deployment.yaml -f postgres.service.yaml  
  
## Redis  
kubectl apply -f redis.configmap.yaml -f redis.deployment.yaml -f redis.service.yaml
  
## Poll  
kubectl apply -f poll.deployment.yaml -f worker.deployment.yaml -f result.deployment.yaml -f poll.service.yaml -f result.service.yaml -f poll.ingress.yaml -f result.ingress.yaml
  
## Traefik  
kubectl apply -f traefik.rbac.yaml  -f traefik.deployment.yaml  -f traefik.service.yaml  
  
## Table sql  
kubectl exec -i deployment/postgres -- psql -U postgres -c "CREATE TABLE IF NOT EXISTS votes (id VARCHAR(255) PRIMARY KEY, vote VARCHAR(255) NOT NULL);"  

## On relance l'app :  
 kubectl apply -f worker.deployment.yaml

## Pour vérifier le statut des conteneurs :
kubectl get pods
Si backoff : problème d'autorisation de l'adresse ip -> changer de réseau.

## Pour véfifier les deploiements :
kubectl get deployments

## Si sur WSL, changer les ports de deploiements :
kubectl port-forward deployment/poll 8080:80
kubectl port-forward deployment/result 8081:80


# Page de vote  :
http://poll.dop.io:30021  
http://localhost:8080

  
# Page de résultats  :
http://result.dop.io:30021  
http://localhost:8081 
  
# Page Traefik  :
http://localhost:30042/dashboard#/