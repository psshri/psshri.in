# DNS config
DNS records are already present in godaddy.com


# authenticate my local with GCP
```
$ gcloud auth login
$ gcloud config list project
$ gcloud config set project psshri-website
$ sudo apt install google-cloud-sdk-gke-gcloud-auth-plugin
$ gcloud container clusters get-credentials psshri-in --zone us-central1-a --project psshri-website
$ kubectl get pods
```

# ssh to GKE node
```
$ gcloud compute ssh --zone "us-central1-a" "gke-psshri-in-primary-node-pool-14fa5f73-12hj" --tunnel-through-iap --project "psshri-website"
```

# docker commands
dockerhub URL: https://hub.docker.com/r/psshri/psshri-in
```
$ docker build -t frontend-psshri-in:v1 .
$ docker tag frontend-psshri-in:v1 psshri/psshri-in:frontend-v1
$ docker push psshri/psshri-in:frontend-v1
$ docker pull psshri/psshri-in:frontend-v1
```

# frontend code
- ran the following command to setup react app
```
$ npx create-react-app frontend
```

# minikube

```
$ minikube start
$ minikube stop
$ minikube delete --all
$ minikube profile list
```

below commands sets minikube to use local docker images
```
$ eval $(minikube docker-env)
$ docker build -t todo-frontend:v2
$ minikube service frontend --url (use this command to access a nodeport service)
```

```
$ minikube update-context
$ minikube start
$ minikube profile list
```

# follow the steps below to run the image locally on minikube

# K8s cluster config
Name: psshri-in
Location type: Zonal (us-central1-a)
Network: default
Node subnet: default (us-central1) (10.128.0.0/20) (4,096 IPA)
Cluster default Pod address range: 192.168.0.0/20 
Service address range: 192.168.16.0/20
External endpoint: enabled
Add your public IP in Control plane authorized networks

Enable image streaming
Enable "Allow access through Google Cloud public IP addresses"

Node type: e2-micro (2 vCPU, 1 core, 1 GB memory)

I can use the following subnets for future use cases, each of them have 4,096 IPA
192.168.0.0/20 (taken)
192.168.16.0/20 (taken)
192.168.32.0/20
192.168.48.0/20
192.168.64.0/20
192.168.80.0/20
192.168.96.0/20
192.168.112.0/20
192.168.128.0/20
192.168.144.0/20


# To explore further
- Kube State Metrics, cAdvisor, Kubelet Metrics



# read later

https://cloud.google.com/kubernetes-engine/docs/how-to/flexible-pod-cidr?authuser=1&hl=en&_gl=1*1rq4x26*_ga*NDg5MDYwMDMzLjE3MTcwMDU0NDY.*_ga_WH2QY8WWF5*MTcyMjAxMjUxOS4yMS4xLjE3MjIwMTc0NDAuNDEuMC4w


# troubleshooting
- If the cluster's IP address is not reachable then your IP might have changed, so update the IP in authorized networks list

# imp points
- static ip for ingress should be global (not regional)

# learnings
- if service of type loadbalancer is used, then publicip:port will give me my website
- if you have your workloads running on a nodepool and then you create another nodepool (the secondary one), and then if you delete the primary node pool, then the workloads will automatically be transferred to the secondary node pool.
- if the image size is too high, it will take a lot of time for the pods to come up. the initial react app was 1GB in size and it took 13 mins for the pod to come up

# lets encrypt certificate via certbot

sudo apt update
sudo apt upgrade
sudo apt install certbot

sudo certbot certonly --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory --manual-public-ip-logging-ok -d psshri.in --email pshekhar930@gmail.com --agree-tos

sudo certbot certonly --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory --manual-public-ip-logging-ok -d blog.psshri.in --email pshekhar930@gmail.com --agree-tos

sudo certbot certonly --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory --manual-public-ip-logging-ok -d www.psshri.in --email pshekhar930@gmail.com --agree-tos

echo "$(sudo cat /etc/letsencrypt/live/psshri.in/privkey.pem)" | base64
echo "$(sudo cat /etc/letsencrypt/live/psshri.in/fullchain.pem)" | base64

echo "$(sudo cat /etc/letsencrypt/live/blog.psshri.in/privkey.pem)" | base64
echo "$(sudo cat /etc/letsencrypt/live/blog.psshri.in/fullchain.pem)" | base64

echo "$(sudo cat /etc/letsencrypt/live/www.psshri.in/privkey.pem)" | base64
echo "$(sudo cat /etc/letsencrypt/live/www.psshri.in/fullchain.pem)" | base64

sudo certbot certificates
sudo certbot renew
sudo certbot revoke --cert-name mydomain.com
sudo certbot delete --cert-name mydomain.com

ref
https://linuxconfig.org/how-to-get-free-ssl-tls-certificates-with-lets-encrypt-and-certbot

curl https://psshri.in -kv
curl -I https://psshri.in 