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

![image](https://github.com/user-attachments/assets/191cf229-fb09-4fce-9eee-114b3203ded0)


 </br> a. Sandbox - A sandbox is an isolated network stack in a container. It includes Ethernet interfaces, ports, routing tables, and DNS config.
 </br> b. Endpoint - An Endpoint joins a Sandbox to a Network.virtual network interfaces (E.g. veth). 
 </br> c. Network -  Networks are a software implementation of a switch (802.1d bridge).

  </br> NetworkController- NetworkController object provides the entry-point into libnetwork that exposes simple APIs for the users (such as Docker Engine) to allocate and manage Networks.
 </br> Libnetwork- Libnetwork is a real-world implementation of the CNM. 
 </br> Drivers- Drivers extend the model by implementing specific network topologies such as VXLAN overlay networks.
 
![image](https://github.com/user-attachments/assets/82357dc6-c82e-48dc-8179-5873a7bf918d)



