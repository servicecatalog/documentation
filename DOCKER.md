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
	
* Connect to the Docker virtual machine again:

	`docker-machine ssh default`
	
ATTENTION: some users trying to set it up on physical machines (a laptop or desktop computer) reported that restarting the Docker virtual machine was not enough, they needed to restart their physical machine. So if it does not work for you, please restart your physical machine!


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

## Configuring the OSCM host and ports

* When running in a Docker container, OSCM is accessible via `http://<DOCKER_HOST>:<PORT>/oscm-portal`

* We recommend setting up `localhost` for `<DOCKER_HOST>`, as described by our [manual](https://hub.docker.com/r/servicecatalog/oscm/) on DockerHub. On Windows, without explicitly mapping for using `localhost` (see how to map at the end of this section), `<DOCKER_HOST>` will be the IP address of the Docker virtual machine, which is visible when starting the Docker Quickstart Terminal, or can be displayed by typing:
	
	`docker-machine ip <machine name>`
  
* OSCM is available at the following ports:

Port | Description
------------ | -------------
8080 | Glassfish HTTP port for accessing the OSCM application 
8081 | Glassfish HTTPS port for accessing the OSCM application 
8048 | Glassfish administration port for the domain hosting the OSCM application 
8480 | Glassfish HTTP port for the domain hosting the OSCM indexer application 
8448 | Glassfish administration port for the domain hosting the OSCM indexer application
It is recommended to assign fixed ports when starting the Docker container:
	
	`docker run -it -p 8080:8080 -p 8081:8081 -p 8048:8048 -p 8480:8480 -p 8448:8448 servicecatalog/oscm`
If you do not explicitly specify ports, Docker will assign random values.

By default the OSCM is configured to be accessed using `localhost` for `<DOCKER_HOST>` and the ports specified in the previous  `docker run` command. If you would like to use the docker host IP address or custom ports, please configure OSCM to use them. For that, please log into `http://<DOCKER_HOST>:<PORT>/oscm-portal` as `administrator/admin123` and change the following OSCM configuration settings accordingly:

Configuration Setting |  New Value
------------ | ------------- | -------------
BASE_URL | `http://localhost:8080/oscm-portal`
BASE_URL_HTTPS | `https://localhost:8081/oscm-portal`
REPORT_ENGINEURL | `http://localhost:8080/birt/frameset?\_\_report\=${reportname}`<br>`.rptdesign&SessionId\=${sessionid}&\_\_locale\=${locale}&WSDLURL\=`<br>`${wsdlurl}&SOAPEndPoint\=${soapendpoint}&wsname=Report&wsport=ReportPort`
REPORT_WSDLURL | `http://localhost:8080/Report/ReportingServiceBean?wsdl`
REPORT_SOAP_ENDPOINT | `http://localhost:8080/Report/ReportingServiceBean`

Since we use Docker on a Windows host machine (and we want to access the Docker virtual machine using a browser located on the Windows host machine), we need to map the `localhost:<PORT>` of the Docker virtual machine to the `localhost:<PORT>` of the Windows host machine. For that, we can use the VBoxManage application contained by the Docker Toolbox, by default located in:
`c:\Program Files\Oracle\VirtualBox>VBoxManage `

Please enter the following commands:

`c:\Program Files\Oracle\VirtualBox>VBoxManage controlvm default natpf1 "rule1,tcp,127.0.0.1,8080,,8080"`
`c:\Program Files\Oracle\VirtualBox>VBoxManage controlvm default natpf1 "rule2,tcp,127.0.0.1,8081,,8081"`
`c:\Program Files\Oracle\VirtualBox>VBoxManage controlvm default natpf1 "rule3,tcp,127.0.0.1,8048,,8048"`
`c:\Program Files\Oracle\VirtualBox>VBoxManage controlvm default natpf1 "rule4,tcp,127.0.0.1,8480,,8480"`
`c:\Program Files\Oracle\VirtualBox>VBoxManage controlvm default natpf1 "rule5,tcp,127.0.0.1,8448,,8448"`

Please also see the [OSCM DockerHub page](https://hub.docker.com/r/servicecatalog/oscm/)!
