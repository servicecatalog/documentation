# Getting Started with ESCM

In a productive environment, different organizations or departments are responsible for managing the ESCM platform, managing marketplaces, publishing services, subscribing to services etc. For a simple demo or testing environment, several sample organizations having the required roles have been created. When an organization is created, a first user is also created having the administrator role for this organization. 

After having successfully deployed ESCM, ESCM can be used to subscribe to a service which will provision an instance in the SUSE OpenStack Cloud (SOC). The following sample organizations and users as well as sample data is available and stored in the ESCM databases: 
 

* **Platform operator** organization with the initial operator user. Whenver ESCM is deployed, this organization is created automatically. The operator works with the ESCM administration portal and can also access the marketplace.

    User name: `administrator`, password: `admin123`

* An organization having the **Technology Provider**, **Supplier** and **Marketplace Owner** roles (`supplierorg`). The administrator can work with the ESCM administration portal as Technology Manager, Service Manager, and Marketplace Manager. He can also access the marketplace.
    
    User name: `supplier`, password: `supplier`

* A **customer** (`customerorg`) organization with a corresponding administrative user having the right to suscribe to services on a marketplace. This is a customer of the `supplier` organization. Customers access ESCM via a marketplace. 

    User name: `customer`, password: `customer`

* A **technical service** (`ESCM_Sample`) which is the basis for a **marketable service** (`ESCM SOC Sample Service`) that the customer organization can subscribe to. 

* A **marketplace** (`Sample Marketplace`) owned by the `supplier` organization. The `ESCM SOC Sample Service` has been published to this marketplace. Before the service could be published to the marketplace, the supplier had to define a price model and activate the service. In our case, it is free of charge for customers

* ESCM provides access to the BIRT **reporting engine**. Depending on the organization and user roles, various reports can be generated using the ESCM administration portal or on the marketplace. 

You can now start playing around with ESCM and work as a user having one of the roles:

* Platform operator
* Technology provider and/or supplier
* Customer

The first user to access ESCM is the platform operator because he is responsible for specifiying a correct email address. 

ENJOY!

## Accessing the ESCM Administration Portal

The ESCM platform is configured using the administration portal which can be accessed using an URL in the following format:

  `https://<hostname.fqdn>:<port>/oscm-portal`

  `<hostname.fqdn>` is the name and the fully qualified domain name of the machine where ESCM has been deployed (the Docker host). 

  `<port>` is the port to address the machine (default: 8081), 

  `oscm-portal` is the default context root of ESCM and cannot be changed.

   In SOC, you can get the base URL of ESCM as follows:
   Open the OpenStack Dashboard on your control node and navigate to `System -> Instances`. There you will see the deployed ESCM instance. **Note its Floating IP address**.
   
   Now you can log in to the ESCM administration portal by addressing the following URL: 
   
   `https://<floating_IP>:8081/oscm-portal`
  
## Working as a Platform Operator

* Login to the administration portal using the operator credentials.

* Go to `Operation --> Update configuration settings` to check if the values for the configuration settings are correct for your environment: 
  
    The most important configuration setting is the following:  
   `BASE_URL_HTTPS`: URL used to access the ESCM administration portal and the marketplace home page (see above). The base URL is provided in emails as a link for accessing ESCM. If you want to persistently change configuration settings, refer to the [Operator's Guide](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/Operation.pdf).
   
* Go to `Account --> Edit profile` to for adding an email address. 

    Enter a valid email address to which notifications from the system are to be sent, and fill in the other mandatory fields for the platform operator organization. 

* Go to `Operation --> Manage users` to check which organizations are already in the database. You should see your own organizationi (`PLATFORM_OPERATOR`) as well as the `supplierorg` and `customerorg` organizations.

  
* Take a tour through the `Operation` menu and have a look at the features. You find detailed information in the [Operator's Guide](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/Operation.pdf).

## Working as a Technology Manager, Service Manager, and Marketplace Owner

The administrator of the `supplierorg` organization can customize a marketplace, register technical services which form the basis of marketable services that are published to marketplaces, create and publish marketable services to a marketplace, register customers, etc.  

* Login to the administration portal using the `supplier` credentials.

* Go to `Technical service --> Export service definition` to take a look at the sample service. You find detailed information on technical services in the [Technology Provider Guide](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/TechProv.pdf). Since the sample service is the basis for provisioning an OpenStack instance, it makes use of the Asynchronous Provisioning Platform (APP) and the OpenStack service controller. For details on the controller, refer to the [OpenStack Integration](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/OSIntegration.pdf) manual.
  

* Go to `Marketable service --> Update service` to take a look at the sample service published to the marketplace. You find detailed information on marketable services in the [Supplier Guide](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/Supplier.pdf). 

* Go to `Marketplace --> Update marketplace` to take a look at the sample marketplace. You find detailed information on marketplaces in the [Marketplace Owner's Guide](https://github.com/servicecatalog/documentation/blob/ESCM/Manuals/MPOwner.pdf). 

* Take a tour through the menus and have a look at the available features. 

* Click the `Go to Marketplace` link above the menus in the administration portal. Select the marketplace and click the `Go to` button. You are redirected to the marketplace portal where the published service is visible. 

    In ESCM, in addition to special roles such as supplier or technology provider, every organization is also assigned the `customer`role. Therefore you can now continue working as a customer. The steps are the same as when logging in with the `customer` credentials (see below).

## Working as a Customer

As customer of the `supplierorg` organization, you have access to the sample marketplace using the `customer` credentials.  

### Accessing the Marketplace

The marketplace is accessed using an URL in the following format:

  `https://<floating_IP>:8081/oscm-portal?marketplaceId=<mid>`

### Subscribing to the Sample Service

On the sample marketplace, you find the `ESCM SOC Sample Service` which is ready for being subscribed to. To do so, you need to log in using the `customer` credentials. Then proceed as follows: 

* Click the `ESCM SOC Sample Service` and then `Get it now`.

* You can enter a name for the subscription. By default, the name is the marketable service name.

* The `ESCM SOC Sample Service` is configured so that all parameters that are mandatory for instantiating an OpenStack instance are already defined. You can view them, and, if required, change them: 

  * Validation pattern for the stack name (regex) in OpenStack
  * Pattern for access information
  * Instance type (flavor)
  * Public network name

* You need to provide values for the following parameters: 

  * OpenStack instance name
  * Image ID: The ID of one of the images defined in SOC
  * OpenStack project ID: The ID of one of the projects defined in SOC 

* Click `Next` and accept the license agreement by clicking `Confirm`. 

    The subscription to the `ESCM SOC Sample Service` is created. It may take some time for asynchronous background tasks to complete, please wait for the subscription be ready. 

* Register yourself and assign you to the subscription once it is ready to be used. 

* You can check in SOC the instantiated OpenStack resources once the sample service has started, and use them for further actions.



## Additional Information

The complete ESCM documentation can be found [here](https://github.com/servicecatalog/documentation/tree/ESCM).  

