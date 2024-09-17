https://labs.iximiuz.com/tutorials/container-networking-from-scratch
https://thenewstack.io/container-networking-landscape-cni-coreos-cnm-docker/


</br>There are two proposed standards for configuring network interfaces for Linux containers:
  </br>The container network model (CNM) 
  </br>The container network interface (CNI)

</br>The container network model (CNM) - The Container Network Model (CNM) is a standard proposed by Docker.
  </br>The CNM is built on 3 main components-
  </br> a. Sandbox
  </br> b. Endpoint
  </br> c. Network
  
![image](https://github.com/user-attachments/assets/5554d785-3e40-416b-9b11-3b01ff76c2dc)

 </br> a. Sandbox - A Sandbox contains the configuration of a container's network stack. which includes container's interfaces, routing table and DNS settings.
  Sandbox can be implemented using  Linux Network Namespace, a FreeBSD Jail or other similar concept.
 </br> b. Endpoint - An Endpoint joins a Sandbox to a Network.
 </br> c. Network - Networks consist of many endpoints.

 </br> NetworkController- NetworkController object provides the entry-point into libnetwork that exposes simple APIs for the users (such as Docker Engine) to allocate and manage Networks

![image](https://github.com/user-attachments/assets/82357dc6-c82e-48dc-8179-5873a7bf918d)



