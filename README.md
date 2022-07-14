# Docker-Same host, custom network connection
Communication between Docker Containers on the same host but different custom networks.

# Establishing Communication between custom networks
While creating a container along with a user-defined network, the following logical network interfaces get created.

1. Network interface of the container (eth*) - Ethernet interface The IP address from which the container is accessible is assigned to this interface.
2. Loopback network (lo) - This loopback network means " only this container"
3. Virtual Ethernet (veth) - Logical cable connecting the eth* of the container with the user-defined bridge.
4. Bridge ( br-xxxxxxxxx) - Link layer software device that forwards traffic between network components via IP addresses. By default, all containers are attached to a bridge called docker0.

While creating two seperate container for Web and Db, connected to seperate bridges, `Bridge-net1` and `Bridge-net2` , communication must be allowed via Routing IP tables.

`sudo iptables -I DOCKER-USER -i br-4eb79ba3670b -o br-710b0a350dcc -j ACCEPT`

 `sudo iptables -I DOCKER-USER -i br-710b0a350dcc -o br-4eb79ba3670b -j ACCEPT`
 
 By adding routes in iptables to accept connection between the bridges, packets of data can flow between Web and Db networks.
 
