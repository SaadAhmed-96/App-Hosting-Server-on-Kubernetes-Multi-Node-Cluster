# Implementing-Nginx-PHP-FPM-and-WordPress-on-a-Multi-Master-Node-Cluster-with-Live-Domain

## Executive Summary:

This report outlines the approach taken to successfully deploy a WordPress website with Nginx, PHP-FPM, and MySQL on a multi-master node Kubernetes cluster, utilizing a live domain (saadcloudways.ml). The solution achieves high availability, security, and scalability while adhering to best practices. Below is a summary of the key steps and configurations involved. The project successfully has utilized key technology like Horizontal Pod Autoscaling (HPA).


## Introduction:

The objective of this project was to deploy a WordPress website on a Kubernetes cluster while utilizing Nginx for the front-end web server, PHP-FPM for serving PHP on the backend, and MySQL (or MariaDB) for the database backend. The deployment was configured to handle a live domain (saadcloudways.ml) and included SSL certificate issuance via cert-manager for secure access. The deployment was conduction on a Cloud Provider with the following **specifications**:

* Ingress-Nginx
* Server: Litespeed
* SSL
* Number of Applications: 1
* Minimum Pods per Application: 2
* Maximum Pods per Application: 10
* Pod Size: 512MB, 1 vCPU
* Node Size: 2GB
* Target CPU for HPA: 80%
* Target Memory for HPA: 80%
* &lt;domain.com>


## Approach:



1. Setting Up Multi-Master Kubernetes Cluster: (Already done in Task1)

The project began by provisioning a multi-master Kubernetes cluster to ensure high availability and fault tolerance.



2. WordPress and MySQL Pods Deployment:

WordPress and MySQL pods were deployed within the cluster. Notably, the MySQL pods were configured with a liveness probe to handle restarts in case of any issues during image downloads.



3. Nginx Configuration for PHP-FPM:

Nginx was configured to forward requests with .php extensions to PHP-FPM via TCP port, ensuring efficient PHP processing for the WordPress application.



4. SSL Certificate Issuance:

An SSL certificate was issued to the live domain (saadcloudways.ml) using cert-manager, implemented via a Helm chart. Initially, Let’s Encrypt's staging API was used to avoid rate limits, and once successful, it was switched to the production API for a valid SSL certificate.



5. Nginx-Ingress Controller Deployment:

The Nginx-Ingress Controller was deployed alongside the WordPress deployment. This controller is configured to use NodePort for external access. The Nginx-Ingress Controller also served as a side-car container within the WordPress pod, enhancing the request routing capabilities.



6. MySQL Service Configuration:

The MySQL service was configured as ClusterIP, limiting external access since it wasn't required for this scenario. This approach enhances security by isolating the database from external connections.



7. Ingress Configuration:

An Ingress resource was configured to route external traffic to the live domain (saadcloudways.ml) to the WordPress deployment within the cluster, ensuring seamless access to the website.



8. Horizontal Pod Autoscaling (HPA):

HPA was implemented with a target CPU and memory utilization of 80%. This ensures that the application scales dynamically based on demand, optimizing resource usage.
