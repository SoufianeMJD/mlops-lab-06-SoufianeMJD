# Compte Rendu - Lab 6 - MLOps

Soufiane MAJDALANE

# **Déploiement K8s d’un système MLOps Churn**

## **Étape 1 : Préparer l’environnement Kubernetes**

**Démarrage Minikube**

![image.png](image%201.png)

**Création du namespace dédié au lab (cluster virtuel)**

![image.png](image%202.png)

**Basculer le contexte courant sur ce namespace**

![image.png](image%203.png)

**Vérification**

![image.png](image%204.png)

## **Étape 2 : Préparer l’image Docker de l’API churn**

Creation d’environment virtuel avec python 3.12

![image.png](image%205.png)

Mise a jour de pip

![image.png](image%206.png)

Installation des dependances

![image.png](image%207.png)

## **Étape 3 : Créer le dossier des manifests Kubernetes**

Creation du dossier k8s

![image.png](image%208.png)

## **Étape 4 : Construire l’image Docker (tag versionné)**

**Construction de l’image Docker avec un tag versionné**

![image.png](image%209.png)

Verification

![image.png](image%2010.png)

## **Étape 5 : Charger explicitement l’image dans Minikube**

**Sauvegarder l’image Docker et Charger l’image dans Minikube**

![image.png](image%2011.png)

**Vérification que l’image est disponible dans Minikube :**

![image.png](image%2012.png)

## **Étape 6 : Deployment Kubernetes pour l’API churn**

Creation du fichier : **k8s/deployment.yaml**

![image.png](image%2013.png)

**Application du manifest :**

![image.png](image%2014.png)

**Suivre le rollout**

![image.png](image%2015.png)

Verification 

![image.png](image%2016.png)

## **Étape 7 : Exposer l’API via un Service NodePort**

Creation du fichier **k8s/service.yaml**

![image.png](image%2017.png)

Application : 

![image.png](image%2018.png)

**Visualisation de la création du Service :**

![image.png](image%2019.png)

**Ouvrir l’accès à la communication avec le cluster**

![image.png](image%2020.png)

Test de l’API

![image.png](image%2021.png)

## **Étape 8 : Injecter la configuration MLOps via ConfigMap**

Creation du fichier **`k8s/configmap.yaml`**

![image.png](image%2022.png)

Application:

![image.png](image%2023.png)

**Affichage du contenu du ConfigMap :**

![image.png](image%2024.png)

Modification de fichier **k8s/deployment.yaml**

![image.png](image%2025.png)

**Réapplication du Deployment** 

![image.png](image%2026.png)

**Suivre le rollout** 

![image.png](image%2027.png)

**Vérification que les variables sont bien injectées dans un Pod**

![image.png](image%2028.png)

## **Étape 9 : Gérer les secrets (MONITORING_TOKEN)**

**Encodage d’une valeur simple en base64**

![image.png](image%2029.png)

YWJjMTIz

Creation du fichier : **`k8s/secret.yaml`**

![image.png](image%2030.png)

Application

![image.png](image%2031.png)

**Affichage des détails**

![image.png](image%2032.png)

Mise a jour de **k8s/deployment.yaml**

![image.png](image%2033.png)

**Réapplication**

![image.png](image%2034.png)

**Vérification du redémarrage et l’état des Pods :**

![image.png](image%2035.png)

**Vérification que la variable d’environnement est bien injectée dans un Pod :**

![image.png](image%2036.png)

## **Étape 10 : Mise en place des endpoints de santé et des probes Kubernetes pour l’API Churn**

**Reconstruction de l’image Docker de l’API :**

![image.png](image%2037.png)

**Exporter l’image et charger l’image dans Minikube :**

![image.png](image%2038.png)

## **Étape 11 : Ajouter les probes (liveness / readiness / startup)**

Mise a jour du **k8s/deployment.yaml**

![image.png](image%2039.png)

**Réapplication et Redéployer l’application dans le cluster Kubernetes :**

![image.png](image%2040.png)

![image.png](image%2041.png)

![image.png](image%2042.png)

## **Étape 12 : Volume persistant pour registry + logs**

Creation du fichier pvc.yaml

![image.png](image%2043.png)

**Application et vérifier que le PVC est bien créé et associé à un volume :**

![image.png](image%2044.png)

Creation du fichier k8s/job-train.yaml

![image.png](image%2045.png)

Application du job

![image.png](image%2046.png)

Mise a jour de **`k8s/deployment.yaml`**

![image.png](image%2047.png)

![image.png](image%2048.png)

**Vérification que les Pods redémarrent correctement :**

![image.png](image%2049.png)

**Vérifier que les dossiers sont bien accessibles depuis un Pod :**

![image.png](image%2050.png)

## **Étape 13 : NetworkPolicy**

Creation du fichier **`k8s/networkpolicy.yaml`**

![image.png](image%2051.png)

**Application de la configuration :**

![image.png](image%2052.png)

## **Étape 14 : Vérifications finales**

**1. Vérification des Pods et des services:**

![image.png](image%2053.png)

**Test de l’endpoint /health :**

![image.png](image%2054.png)

![image.png](image%2055.png)

![image.png](image%2056.png)

Liste des Pods

![image.png](image%2057.png)

churn-api-5b7bb7f59-2gbnz   1/1     Running   0          36m
churn-api-5b7bb7f59-d4hz8   1/1     Running   0          32m

**Exécution de la détection de drift dans le Pod :**

![image.png](image%2058.png)
