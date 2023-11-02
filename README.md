# Implementing-Nginx-PHP-FPM-and-WordPress-on-a-Multi-Master-Node-Cluster-with-Live-Domain

## <span style="text-decoration:underline;">Executive Summary:</span>

This report outlines the approach taken to successfully deploy a WordPress website with Nginx, PHP-FPM, and MySQL on a multi-master node Kubernetes cluster, utilizing a live domain (saadcloudways.ml). The solution achieves high availability, security, and scalability while adhering to best practices. Below is a summary of the key steps and configurations involved.


## <span style="text-decoration:underline;">Introduction:</span>

The objective of this project was to deploy a WordPress website on a Kubernetes cluster while utilizing Nginx for the front-end web server, PHP-FPM for serving PHP on the backend, and MySQL (or MariaDB) for the database backend. The deployment was configured to handle a live domain (saadcloudways.ml) and included SSL certificate issuance via cert-manager for secure access.


## <span style="text-decoration:underline;">Approach:</span>



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


## Results:



1. Live-Domain loading Website (**saadcloudways.ml)**: 

<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")

2. Wordpress along with MySQL pods being ran:

    **Note**: The “1” restart shown among mysql pods happened due to “Liveness Probe” attached. The MySQL image didn’t get downloaded within the grace period. Hence, a restart was made.


    

<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


3. Nginx configuration made to forward any requests with .php extension to PHP-FPM TCP port:

    

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")
 

4. SSL certificate issued to the domain via cert-manager implementation using helm chart:

    

<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


5. Issued Let’s Encrypt first using Let’s encrypt staging api to avoid rate limit incase of failures. Once, SSL got issued successfully then the api URL was changed to production:

    

<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")


6. The Nginx-Ingress Controller running along side with wordpress deployment (which contains Nginx+FPM as side-car) on NodePort & SQL deployment on ClusterIP (as we don’t need to access the MySQL service from outside on this scenario) : 

<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")
Lastly, Ingress pointing towards my domain: 

<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")
