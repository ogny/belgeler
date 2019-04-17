her task'a client kurarak service discovery sagliyor

##### Service Discovery with Docker and Consul: part 1
```bash

docker-machine create nb-consul --driver virtualbox

docker run -d		\
    --restart always	\
    -p 8300:8300	\
    -p 8301:8301	\
    -p 8301:8301/udp	\
    -p 8302:8302/udp	\
    -p 8302:8302	\
    -p 8400:8400	\
    -p 8500:8500	\
    -p 53:53/udp	\
    -h server1		\
    progrium/consul	\
    -server		\
    -bootstrap		\
    -ui-dir /ui		\
    -advertise $(dm ip nb-consul)


docker-machine create -d virtualbox							\
	    --swarm --swarm-master							\ 
	    --swarm-discovery="consul://$(docker-machine ip nb-consul):8500"		\
	    --engine-opt="cluster-store=consul://$(docker-machine ip nb-consul):8500"	\
	    --engine-opt="cluster-advertise=eth1:2376" nb1
 
docker-machine create -d virtualbox							\
	    --swarm									\  
            --swarm-discovery="consul://$(docker-machine ip nb-consul):8500"		\
            --engine-opt="cluster-store=consul://$(docker-machine ip nb-consul):8500"	\
	    --engine-opt="cluster-advertise=eth1:2376" nb2
 
docker-machine create -d virtualbox							\ 
	    --swarm									\
	    --swarm-discovery="consul://$(docker-machine ip nb-consul):8500"		\
	    --engine-opt="cluster-store=consul://$(docker-machine ip nb-consul):8500"	\
	    --engine-opt="cluster-advertise=eth1:2376" nb3
```


`docker service create --name <serviceName> --replicas=10 --network myNetwork --constraint 'node.labels.webservice == true'  --publish 80:9000 <image>`
