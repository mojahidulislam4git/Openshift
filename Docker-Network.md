https://labs.iximiuz.com/tutorials/container-networking-from-scratch
https://thenewstack.io/container-networking-landscape-cni-coreos-cnm-docker/


</br>There are two proposed standards for configuring network interfaces for Linux containers:
  </br>The container network model (CNM) 
  </br>The container network interface (CNI)

</br>The container network model (CNM) - The Container Network Model (CNM) is a standard proposed by Docker.
  </br>The CNM is built on 3 main components-
  </br> a. Sandbox - A sandbox is an isolated network stack in a container. It includes Ethernet interfaces, ports, routing tables, and DNS config.
  </br> b. Endpoint - An Endpoint joins a Sandbox to a Network.virtual network interfaces (E.g. veth). 
  </br> c. Network - Networks are a software implementation of a switch (802.1d bridge).
  


</br> NetworkController- NetworkController object provides the entry-point into libnetwork that exposes simple APIs for the users (such as Docker Engine) to allocate and manage Networks.
</br> Libnetwork- The CNM is the design doc and libnetwork is the canonical implementation.
</br> Drivers- Drivers extend the model by implementing specific network topologies such as VXLAN overlay networks.

<img src="https://github.com/user-attachments/assets/191cf229-fb09-4fce-9eee-114b3203ded0" width="300">
<img src="https://github.com/user-attachments/assets/3a3fe48c-968e-4660-aa4a-b01aea24523a" width="400">
<img src="https://github.com/user-attachments/assets/82357dc6-c82e-48dc-8179-5873a7bf918d" width="200">

 </br>Docker ships with several built-in drivers, known as native drivers or localdrivers-
 </br>a. bridge 
 </br>b. host
 </br>c. none
 </br>d. overlay
 </br>e. macvlan 
 </br>f. ipvlan
  
</br>a. Bridge - (Default) A bridge network uses a software bridge which lets containers connected to the same bridge network communicate, while providing isolation from containers that aren’t connected to that bridge network.
</br>b. Host - The Host network driver in Docker is a networking mode that allows containers to directly use the networking stack of the Docker host machine.
</br>c. none - The none driver simply disables networking for a container, making it isolated from other containers.
</br>d. overlay - The overlay network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to it to communicate securely when encryption is enabled.
</br>e. macvlan - Macvlan network is used to connect applications directly to the physical network. By using the macvlan network driver to assign a MAC address to each container, also allow having full TCP/Ip stack. Then, the Docker daemon routes traffic to containers by their MAC addresses. 
</br>f. ipvlan - The IPvlan driver gives users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration.

<img src="https://github.com/user-attachments/assets/d53bba7e-18d7-4cff-9ca1-94da6baca8b8" width="200">
<img src="https://github.com/user-attachments/assets/4c37983e-10dc-45ac-8117-8cb8acafb8c2" width="150">
<img src="https://github.com/user-attachments/assets/36a5d5b5-9faf-46ea-952c-3f5983e5ee0f" width="175">
<img src="https://github.com/user-attachments/assets/38e2567f-1c32-4a2c-b81f-a5884b34821e" width="150">
<img src="https://github.com/user-attachments/assets/beec175b-a0b1-4fea-be45-8a370ea45a3a" width="150">
<img src="https://github.com/user-attachments/assets/6b333811-d601-4f7d-bc09-cf68fd686079" width="150">

</br>Service discovery-Service discovery allows all containers and Swarm services to locate each other by name. The only requirement is that they be on the same network.this leverages Docker’s embedded DNS server and the DNS resolver in each container.
</br>Ingress load balancing- Swarm supports two network publishing modes that make services accessible outside of the cluster-
  </br>Ingress mode (default) -  Services published via ingress mode can be accessed from any node in the Swarm — even nodes not running a service replica. 
  </br>Host mode -  Services published via host mode can only be accessed by hitting nodes running service replicas.

  </br>Lab - [bridge-utils/brctl]
 </br> $ docker network ls 
 </br> $ docker inspect bridge
 </br> $ ip link show docker0 
 </br> $ docker inspect bridge | grep bridge.name 
 </br> $ docker network create -d bridge localnet
 </br> $ brctl show 
 </br> $ docker run -d --name c1 --network localnet alpine sleep 1d 
 </br> $ docker inspect localnet --format '{{json .Containers}}' 
 </br> $ docker run -it --name c2 --network localnet alpine sh 
 </br> $ docker run -d --name web --network localnet --publish 5001:80 nginx
 </br> $ docker port web 
 </br> $ docker network create -d macvlan --subnet=10.0.0.0/24 --ip-range=10.0.0.0/25 --gateway=10.0.0.1 -o parent=eth0.100 macvlan100 
 </br> $ docker run -d --name mactainer1 --network macvlan100 alpine sleep 1d 
 </br> $ docker run -it --name c1 --dns=8.8.8.8 --dns-search=nigelpoulton.com alpine sh
</br>  $ docker service create -d --name svc1 --publish published=5001,target=80,mode=host nginx 

</br>  $ docker network create -d overlay uber-net 
</br>  $ docker service create --name test --network uber-net --replicas 2 ubuntu sleep infinity 



======================================================
Container Network Interface (CNI)-
Container Network Interface (CNI) is a framework for dynamically configuring networking resources. It uses a group of libraries and specifications written in Go.
consists of a specification and libraries for writing plugins to configure network interfaces in Linux containers, along with a number of plugins.
Kubernetes uses CNI as an interface between network providers and Kubernetes pod networking.


CNI network providers implement their network fabric using either an encapsulated network model such as Virtual Extensible Lan (VXLAN) or an unencapsulated network model such as Border Gateway Protocol (BGP).

Encapsulated Network - 
This network model generates a kind of network bridge extended between Kubernetes workers, where pods are connected.
This network model is used when an extended L2 bridge is preferred. This network model is sensitive to L3 network latencies of the Kubernetes workers. 
![image](https://github.com/user-attachments/assets/28fe3f3a-6e21-4a70-aa07-bcba621895db)

   
 

