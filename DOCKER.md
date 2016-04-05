<p align="center"><h1><img height="52" src="https://avatars0.githubusercontent.com/u/14330878" alt="Open Service Catalog Manager"/>&nbsp;OSCM Docker image</h1></p> 

Welcome to the documentation of Open Service Catalog Manager (OSCM)!

## Configuring Docker machine to work with corporate proxy
To setup your proxy (skip if pulling the OSCM image succeeded):

* Enter the following commands (please replace <proxy_host> and <proxy_port> with the host and port values of the active proxy in your network first. To obtain your proxy configuration, please contact your network administrator.):

sudo touch /var/lib/boot2docker/profile

sudo echo "export HTTP_PROXY=<proxy_host>:<proxy_port>" | sudo tee /var/lib/boot2docker/profile

sudo echo "export HTTPS_PROXY=<proxy_host>:<proxy_port>" | sudo tee -a /var/lib/boot2docker/profile

* Exit the Docker virtual machine by typing exit.

* Restart the Docker virtual machine by typing:
docker-machine stop default
docker-machine start default

ATTENTION: some users trying to set it up on local machines (a laptop or desktop computer) reported that restarting the Docker virtual machine was not enough, they needed to restart their physical machine. So if it does not work for you, please restart your physical machine!

* Connect to the Docker virtual machine again:
docker-machine ssh default

* Pull the OSCM Docker image from DockerHub to your Docker virtual machine:
docker pull servicecatalog/oscm

* Another way to achieve the same result is to: 
 - delete your existing Docker machine:
	docker-machine stop default
	docker-machine rm default
 - create new machine with proxy parameter:
	docker-machine create -d virtualbox \
    --engine-env HTTP_PROXY=http://example.com:8080 \
    --engine-env HTTPS_PROXY=https://example.com:8080 \
    --engine-env NO_PROXY=example2.com \
    default
 - start your new machine:
	docker-machine start default

## Configuring Docker host and ports

* Docker image is accessible via http://DOCKER_HOST:PORT/oscm-portal

* If you follow our [manual](https://hub.docker.com/r/servicecatalog/oscm/) on DockerHub, we advise to use localhost as DOCKER_HOST.

* Without explicitly changing to localhost, Docker will use the IP address of the virtual machine. 
It is visible when starting the Docker Quickstart Terminal, or can be displayed by typing:
docker-machine ip <machine name>
  
* Docker ports can be changed when running our image:

	docker run -it -p 18080:8080 -p 18081:8081 -p 18048:8048 -p 18480:8480 -p 18448:8448 servicecatalog/oscm
	
Ports 18080, 18081, 18048, 18480 and 18448 can be changed as you like. If you do not explicitly specify ports, Docker will assign random values.
	
Below you can find the ports which are exposed by running the servicecatalog/oscm image:

Port | Description
------------ | -------------
8080 | Glassfish HTTP port for accessing the OSCM application 
8081 | Glassfish HTTPS port for accessing the OSCM application 
8048 | Glassfish administration port for the domain hosting the OSCM application 
8480 | Glassfish HTTP port for the domain hosting the OSCM indexer application 
8448 | Glassfish administration port for the domain hosting the OSCM indexer application
