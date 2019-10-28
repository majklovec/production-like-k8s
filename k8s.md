
# K8s

## What is it?

Ochestrator of containers - cluster

Abstracts away the underlying sw
HA
apps on many servers
roles in cluster

## Concepts

### Pod - main building block. Pods are mortal. Non stable network iface

### Deployment - update of Pods
contains Pod & Replica sets

### Service
immortal IP or DNS name for selected Pods

### Ingress 
- external service using domain

### Cluster components
API - backed by etc
Controller Manager - ensures actual state of the cluster

### Scheduler - bindw pod to node
### Kubelet - client for API server runs Pods (on pods)
Kuer Proxy - forwards traffic into cluster

kubeadm
kubectl
minikube
kops - reate cloud

Workload
Pods controllers -

sika.link/k8s

kubectl get nodes

Terraform - neco jako vagrant

KUBECONFIG=....

kubectl explain node
kubectl proxy -p 8000 udela proxy API z k8s serveru na local

deploy app:
kubectl apply -f ./01_pod.yml 

reuest - kolik chci
limit - kolik dostane jako docker
cpu - 20m - 20 mili/cpu = 2%

 1118  kubectl apply -f ./01_pod.yml 
 1119  kubectl get -f ./01_pod.yml 
 1120  kubectl get nodes
 1121  kubectl describe nodes minikube
 
 more info about nodes
 kubectl get nodes -o wide 

detailed info about the pod
kubectl describe -f ./01_pod.yml 

kubectl apply -f 01_pod.yml -f 02_pod.yml -n namespace

kubectl create ns sssss
kubectl get namespaces
kubectl get nodes --show-labels



Node - server
Pod - group of dockers
Service - reverzni proxy
ReplicaSet - udrzuje pocet replik, managuje Pody - pridava neodebira, pri downscale nevis co smaze a co zustane
Deploy - managuje ReplicaSety, zmeni napr image vsech podu
DeamonSet - uzdrzuje pod na node, 1 per node, klidne i na vice nodech vzdy jedna instance - napr. monitoring kazdeho node, logger ...
StateFulSets - deploy rady replik, cislovane pody, je jiste ze pri downscale se odebiraji od konce - tj. resi poradi podu a jejich odebirani, spousti pody   (redis)

Job - neco jako Pod, jednorazova akce, napr CI build - po dobehnuti skriptu zustane shnilej Job, kterej zmizi po 3mins
CronJob - pousti joby podle cronu

Run Pod
kubectl run -it --rm --image=debian --generator=run-pod/v1 my-debian -- bash


Services
- ClusterIP - publikace na vnitrni IP - v ramci clusteru
- NodePort - publikace portu primo na vsech nodes, vysoky porty > 30000
- Loadbalancer - jen tam kde je pred tim neco co si umi s K8s povidat (AWS, F5, ...), pro bare metal asi ku prdu

kubectl delete --all all

kubect create namespace jmenonamespace

Deploy do daneho namespace
kubectl apply -f .... -n namespace

Debug
kubectl get ....

Ingress

minikube delete && minikube start

Storage

EmptyDir
- je na node
- maze se se smazanim podu + se zmeni uid, takze jina cesta
- muze byt ramdisk
- vhodne na cache ne jako persistentni disk


PersistenVolume 
- definice volume
- dostupny z clusteru

PersistenVolumeClaim 
- pouziva volumes
- dostupny z namespace
- ReadWriteOnce, ReadWriteMany - mam pristup jen ja nebo je sdileny
- kdyz nema PersitentVlume, vytvoriho podle default storage class

- claim policy: retain, delete


Rook.io  strage provider
minio - S3
CEPH  - storage


Kubernetes na vlastnim hw
https://github.com/ondrejsika/bare-metal-kubernetes
Tiller = helm
kubeadm token
kubernetes dashboard 

jak multimaster?

ConfigMaps
Pro rychlejsi predani configu - misto env promennych

Secret je jako configmapa, secret pouziva i k8s interne
Jinak je to stejny, secret spis pro hesla configmaps spis pro config obecne.

Jdou pripoji jako env, jako soubory a jako dirs
Wault


Prava - RBAC
kubectl api-resources -o wide

2 druhy roli
ClusterRole - globalni 
Role - v ramci namespace

kubectl get clusterrole

Switch mezi contextama
kubectl config use-context do

## Probes

LivenessProbe - bezi nebo otocit?
Readiness - da se pripojit 
StartupProbe - uz se da pripojit do LoadBalanceru

3 druhy spousteni probe
- command
- http dotaz na endpoint (/healtx, /livez)
- check portu - tcp ocket

InitContainer - spusti se podle poradi pred vlastnim podem




Autoscaling (Horizontal Pod Autoscaler)
Metrics server



Dotazy:
Jde limitovat namespace? 
- jo = LimitRange

kubectl aoiversion=scaling/apiv2beta

minikube service xxx --url


kubectl patch service xxxx - p ' '{"spec": {"seector ...... }"


Strategie deploymentu
Ramps - pusti danej pocet killne danej pocet a tak dokola



Helm
- package manager

https://github.com/helm/charts