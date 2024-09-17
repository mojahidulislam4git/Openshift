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

![image](https://github.com/user-attachments/assets/e7cecd02-5a18-4f18-964d-545ac9f8304c)

 </br> a. Sandbox - A Sandbox contains the configuration of a container's network stack. which includes container's interfaces, routing table and DNS settings.
  Sandbox can be implemented using  Linux Network Namespace, a FreeBSD Jail or other similar concept.
 </br> b. Endpoint - An Endpoint joins a Sandbox to a Network.
 </br> c. Network - Networks consist of many endpoints.

  </br> NetworkController- NetworkController object provides the entry-point into libnetwork that exposes simple APIs for the users (such as Docker Engine) to allocate and manage Networks.
 </br> Libnetwork- Libnetwork is a real-world implementation of the CNM. 
 </br> Drivers- Drivers extend the model by implementing specific network topologies such as VXLAN overlay networks.
 

![image](https://github.com/user-attachments/assets/82357dc6-c82e-48dc-8179-5873a7bf918d)



