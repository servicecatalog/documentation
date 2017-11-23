# ESCM Deployment Guide

FUJITSU Software Enterprise Service Catalog Manager (ESCM) is cloud services management software, licensed as &quot;OpenStack Compatible&quot;. It offers ready-to-use service provisioning adapters for cloud service providers like Amazon Web Services (AWS) and OpenStack, but is also open for integrating other platforms.

The ESCM software is available as Docker images, deployed as a cloud application using Heat templates and configured to interact with the SUSE OpenStack Cloud.

The complete documentation can be found [here](https://github.com/servicecatalog/documentation/tree/ESCM).

TODO: where to find more about escm docker containers, insert link

## 1. Prerequisites

#### OpenStack Services

For installing ESCM in SUSE OpenStack Cloud, the following OpenStack services must be already installed and running:

- Keystone
- Nova
- Neutron
- Glance
- Cinder
- Heat

#### SLES Image

The ESCM barclamp expects a SLES image in the Glance image registry of the SUSE OpenStack Cloud. The image must contain Docker and Docker Compose. A ready-to-use image in qcow2 format can be downloaded from SUSE Studio Express (TODO Link) and imported in glance as a public image with the image name sles12-sp3.

#### Network Settings

Be aware that ESCM uses external hosts (mail, proxy, Docker registry) at deployment or run time. Depending on the environment, you can specify an IP address, host name or FQDN for the host setting.

#### Mail Notification

ESCM relies on an external mail server for mail notification. This setting is mandatory.

## 2. ESCM OpenStack Resources

The ESCM barclamp creates instance and volume stacks using predefined Heat templates. The ESCM Docker containers are hosted on the provisioned instance and use Cinder volumes for persisting data (logs and data).

The settings for the Heat templates are not exposed in the barclamp graphical interface, but available only in _Raw_ mode:

 ![](/Deployment Guide_files/image001.png)

An exception is the public part of the OpenStack SSH keypair for accessing the instance. The keypair is available in the graphical interface. An OpenStack SSH keypair with the specified public part will be created and assigned to the instance, so you can access it later using the corresponding private key. Setting a public key is mandatory.

OpenStack Settings: Public Key

![](/Deployment Guide_files/image002.png)


## 3. ESCM Settings

The ESCM settings are available in the barclamp graphical interface (_Custom_ mode).

#### Mail Settings

**Mail Host** : Host of the mail server to be used

**SMTP Port** : SMTP port of the mail server

**Enable TLS** : true if the mail server requires a secure connection, false otherwise

**ESCM Email Address** : The email address used by ESCM for sending email notifications

**Authentication Required** : true if the server requires authentication, false otherwise

**User** : User name for SMTP authentication

**Password** : Password for SMTP authentication

![](/Deployment Guide_files/image003.png)


#### Docker Registry

DockerHub is configured as a registry for the ESCM software by default. If it is necessary to use a custom registry, its configuration can be specified. Depending on the registry, authentication can also be configured.

**Use Docker Hub** : true if you use the official ESCM Docker images as provided on Docker Hub, false otherwise.

**Authentication Required** : true if the registry requires authentication, or if you want to use authentication, false otherwise.

**User** : Login registry authentication

**Password** : Password for registry authentication

**Organization** : Relevant only when using a custom registry. Specifies the organization which contains the ESCM images in the registry.

**Registry Host** : Docker registry host

**Registry Port** : Docker registry port

![](/Deployment Guide_files/image004.png)


#### Proxy Settings

Depending on the environment, it may be necessary to specify proxy settings for the OpenStack instance hosting the ESCM Docker images.

These settings are used for the Docker engine environment which needs access to the Docker registry and ESCM Docker containers which access the internet.

**HTTP Proxy Host** : The HTTP proxy host

**HTTP Proxy Port** : The HTTP proxy port

**HTTPS Proxy Host** : The HTTPS proxy host

**HTTPS Proxy Port** : The HTTPS proxy port

**No Proxy** : Host names, IP addresses or FQDNs for which the proxy should be bypassed in a comma separated list. This already contains localhost, 127.0.0.1 and the floating IP address of the instance by default. Set additional ones if necessary.

**Authentication Required** : true if the proxy server requires authentication, false otherwise

**User** : User name for proxy authentication

**Password** : Password for proxy authentication

![](/Deployment Guide_files/image005.png)


#### SSL Settings

You must specify the locations for the certificate key pair files which the application will use for its HTTPS listeners. Note that both trusted and self-signed certificates are accepted.

**Protocol** : HTTPS (ESCM always uses HTTPS)

**Generate self-signed certificates** : true if you wish to let the system automatically generate and use self-signed certificates, false if you wish you provide your own certificate and key files.

**Host FQDN/IP address** : Automatic certificates will be generated with the floating IP address of the instance as the Common Name. You can override the floating IP address with your own IP address or host name.

**SSL Certificate File** : Location of your SSL public certificate file on the OpenStack Control Node, if you wish to provide your own. The certificate must be in PEM format.

**SSL Private Key File** : Location of your SSL private key file on the OpenStack Control Node, if you wish to provide your own. The file must be in PEM format.

**SSL CA Certificates File** : Location of your SSL intermediates certificate (&quot;chain&quot;) file on the OpenStack Control Node, if you wish to provide your own. The certificate must be in PEM format. This setting is optional.

![](/Deployment Guide_files/image006.png)


## 4. Operations and Troubleshooting

After having applied the ESCM proposal, it may take some minutes until the ESCM services are available, although a message might be displayed that ESCM has been deployed successfully. You may log in to the ESCM instance where the ESCM containers are running at any time, either to do operational tasks or to check the status. You may use the floating IP of the instance and the OpenStack SSH public key you provided.

##### How to access the ESCM instance

TODO

##### How to see the deployment progress

TODO

##### Where to find the ESCM logs

TODO

##### How to backup/restore the ESCM database

TODO

....

