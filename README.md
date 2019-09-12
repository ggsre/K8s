### Docker vs K8s  
Docker  
- Containers can Node1 cannot communicate with containers on Node2
- All containers on a node share the host IP space & must coordinate which ports they use on that node  
- If a container must be replaced, any hard-coded IP addresses will break  
 
K8s  
- All containers can communicate with other containers without NAT   
- All nodes can communicate with other containers (and vice versa) without NAT   
- The IP that the container sees itself as is the IP that others see it as   
