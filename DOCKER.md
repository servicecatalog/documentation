<p align="center"><h1><img height="52" src="https://avatars0.githubusercontent.com/u/14330878" alt="Open Service Catalog Manager"/>&nbsp;OSCM Docker image</h1></p> 

Welcome to the documentation of OSCM Docker image!
The following description is intended also for people without previous Docker experience and is exemplified using Docker Toolbox on a Windows host machine.

## Configuring the Docker machine to work with a proxy
To setup your proxy (skip if pulling the OSCM image succeeded):

* Enter the following commands (please replace `<proxy_host>` and `<proxy_port>` with the host and port values of the active proxy in your network first. To obtain your proxy configuration, please contact your network administrator.):
`sudo touch /var/lib/boot2docker/profile`
`sudo echo "export HTTP_PROXY=<proxy_host>:<proxy_port>" | sudo tee /var/lib/boot2docker/profile`
`sudo echo "export HTTPS_PROXY=<proxy_host>:<proxy_port>" | sudo tee -a /var/lib/boot2docker/profile`

* Exit the Docker virtual machine by typing exit.
* Restart the Docker virtual machine by typing:
`docker-machine stop default`
`docker-machine start default`
ATTENTION: some users trying to set it up on physical machines (a laptop or desktop computer) reported that restarting the Docker virtual machine was not enough, they needed to restart their physical machine. So if it does not work for you, please restart your physical machine!
* Connect to the Docker virtual machine again:
`docker-machine ssh default`

Another way to achieve the same result is to: 
* delete your existing Docker virtual machine:
	`docker-machine stop default`
	`docker-machine rm default`
* create a new Docker virtual machine with proxy parameter:
	`docker-machine create -d virtualbox --engine-env HTTP_PROXY=http://example.com:8080 --engine-env HTTPS_PROXY=https://example.com:8080 --engine-env NO_PROXY=example2.com default`
* start your new Docker virtual machine:
	`docker-machine start default`
* Connect to the new Docker virtual machine:
`docker-machine ssh default`

## Configuring Docker host and ports

* Docker image is accessible via http://DOCKER_HOST:PORT/oscm-portal

* If you follow our [manual](https://hub.docker.com/r/servicecatalog/oscm/) on DockerHub, we advise to use localhost as DOCKER_HOST.

* Without explicitly changing to localhost, Docker will use the IP address of the virtual machine. 
It is visible when starting the Docker Quickstart Terminal, or can be displayed by typing:

`docker-machine ip <machine name>`
  
* Docker ports can be changed when running our image:

	`docker run -it -p 18080:8080 -p 18081:8081 -p 18048:8048 -p 18480:8480 -p 18448:8448 servicecatalog/oscm`
	
Ports 18080, 18081, 18048, 18480 and 18448 can be changed as you like. If you do not explicitly specify ports, Docker will assign random values.
	
Below you can find the ports which are exposed by running the servicecatalog/oscm image:

Port | Description
------------ | -------------
8080 | Glassfish HTTP port for accessing the OSCM application 
8081 | Glassfish HTTPS port for accessing the OSCM application 
8048 | Glassfish administration port for the domain hosting the OSCM application 
8480 | Glassfish HTTP port for the domain hosting the OSCM indexer application 
8448 | Glassfish administration port for the domain hosting the OSCM indexer application

## Configuring OSCM
Before you start using the OSCM, some additional configuration steps are necessary to adapt the OSCM to your Docker environment:

To ensure the communication between the OSCM components, please make sure OSCM knows the `<DOCKER_HOST>` and `<PORT>`. For that log in as *administrator* and change the following OSCM configuration settings. Please make sure you use the correct ports: the port Docker mapped for 8080 for HTTP URLs and the port mapped for 8081 for HTTPS URLs (set it to 18080 and 18081 respectively, unless you provided your own values during `docker run` command).

Configuration Setting |  New Value
------------ | ------------- | -------------
BASE_URL | `http://localhost:18080/oscm-portal`
BASE_URL_HTTPS | `https://localhost:18081/oscm-portal`
REPORT_ENGINEURL | `http://localhost:18080/birt/frameset?\_\_report\=${reportname}`<br>`.rptdesign&SessionId\=${sessionid}&\_\_locale\=${locale}&WSDLURL\=`<br>`${wsdlurl}&SOAPEndPoint\=${soapendpoint}&wsname=Report&wsport=ReportPort`
REPORT_WSDLURL | `http://localhost:8080/Report/ReportingServiceBean?wsdl`
REPORT_SOAP_ENDPOINT | `http://localhost:8080/Report/ReportingServiceBean`

[OSCM DockerHub page](https://hub.docker.com/r/servicecatalog/oscm/)
