# Energy-Data-Space-Connector

## Introduction
<img src="images/TRUE_Connector_Logo.png" alt="True Connector" width="25%" height="25%">
<img src="images/idsa-sign-component-certification-small.png" alt="IDS certified" width="5%" height="5%">

This project started from and extends the [Engineering True Connector](https://github.com/Engineering-Research-and-Development/true-connector), a general purpose Data Space Connector and open-source project developed by ENG. The connector is an open-source solution, designed to enable self-determined data sharing while ensuring compliance with regulations such as GDPR. Initially focused on the manufacturing domain, the TRUE Connector has proven its versatility across diverse sectors including circular economy, energy, smart buildings, and agri-food domains. It has recently received IDS certification.

<br />

<img src="images/OneNet.svg" alt="OneNet Project" width="15%" height="15%">

Furthermore, the project builds upon the work carried out with the [OneNet connector](https://github.com/european-dynamics-rnd/OneNet), developed within the [OneNet project](https://www.onenet-project.eu/). Specifically, the following elements were adopted and extended from the OneNet connector: 

* the OneNet Middleware for centralized services such as Identity Management and Service Catalogue (extended)
* the Semantic Vocabulary with more than 60 standardized services for the energy domain

## New Features 

This version of the Energy Data Space Connector includes:

* New open-source advanced GUI 
* Blockchain Notarization and Verification for data transactions
* Integration of External Service via REST APIs and Push mechanisms
* NATS protocol integration through the NATS Component



## Prerequisites
The deployment process involves the use of Docker containers. The use of Docker guarantees not only an easy deployment process and total portability of the solution, but also a high level of scalability of the released applications.
The hardware and operating system prerequisites are:
* A 64bit 2-core processor
*	8GB RAM Memory
*	50GB of disk space or more

The software prerequisites include:
*	Linux or Windows (preferably Server edition) Operative System (OS);
*	docker and docker-compose;

Energy Data Space Connector software and its components will be delivered utilizing the Docker containers functionalities. Firstly, the Docker platform has to be downloaded and installed accordingly to the OS of the server to host the deployment.
For the correct installation of docker and docker-compose, please refer to the official guides: https://docs.docker.com/get-docker/

## Energy Data Space Connector installation on Docker
To proceed with the installation of Energy Dataspace Connector, the user must use the docker folder of the github repository that contains all the necessary configuration.

1.	The first step is to clone this repository https://github.com/Horizont-Europe-Interstore/Data-Space-Connector in a specific folder *energy-data-space-connector*, by typing:
```
mkdir energy-data-space-connector
cd energy-data-space-connector
git clone https://github.com/Horizont-Europe-Interstore/Data-Space-Connector.git
```

2.	There is the *docker-compose.yml* file located under the docker folder that contains all the configuration of the Energy Data Space Connector containers. Go to that folder by typing the command:
```
cd energy-data-space-connector/docker
```

3.	Start the containers with the below command:
```
docker compose up -d --build
```
The default configuration, recommended for connector testing, simulates a complete environment with 2 connectors within the same docker.
If, however, you want use the connector in a real environment, or test it with 2 indipendent machine, please read section [Deploy a single connector instance](#deploy-a-single-connector-instance).

4.	To show logs use the command:
```
docker-compose logs -f
```
Alternatively you can use dozzle UI to access the logs of each container. Open the following url on your browser :
```
http://localhost:8081
```

5.	If no errors are seen, this means that Energy Data Space Connector was successfully deployed on your premisses.

To stop all the containers use:
```
docker compose down
```

### Hints
#### Deploy a single connector instance
The configuration present within the docker-compose.yml simulates a complete environment with 2 connectors.
By starting this configuration both connectors are started within the same docker.

If, however, you want use the connector in a real environment, or test it with 2 indipendent machine, you can deploy a single connector instance.
For this purpose, an additional docker-compose file (*docker-compose-single.yml*) is provided that launches a single connector instance.
You can start this configuration using the commands:
```
docker compose -f docker-compose-single.yml up -d --build
```
and stop with:
```
docker compose -f docker-compose-single.yml down
```
This configuration is recommended for installing a connector production node because it allows you to launch a single connector instance and save hardware resources.

#### Optional Nginx configuration
Optionally you can use nginx as proxy.

The folder _docker/nginx-connector-config/_ contains a [_docker-compose.yml_](docker/nginx-connector-config/docker-compose.yml) file and an example of nginx configuration ([_nginx.conf_](docker/nginx-connector-config/nginx.conf)).

The [_docker-compose.yml_](docker/nginx-connector-config/docker-compose.yml) defines the nginx service to start (from the official image), the exposed ports and the volumes to mount.

```
services:
  nginx:
   image : nginx:latest
   ports :
       - "8080:8080"
       - "80:80"
       - "443:443"
   volumes:
       - ./ssl:/etc/nginx/ssl
       - ./nginx.conf:/etc/nginx/conf.d/default.conf
``` 

The [(_nginx.conf_)](docker/nginx-connector-config/nginx.conf) contains header management, SSL configuration and location directives necessary for the correct functioning of the connector.

In particular, for each service to be exposed, a location type directive must be defined with the following configurations (please replace the _**path**_ and _**uri**_ placeholders with your own values):
```
  location /<path> {
    proxy_pass <uri>;
    proxy_redirect off;
    proxy_set_header    Upgrade     $http_upgrade;
    proxy_set_header    Connection  "upgrade";
    proxy_set_header Host $host:$server_port;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Ssl on;
    proxy_set_header  X-Forwarded-Proto  https;
    rewrite ^/<path>/(.*)$ /$1 break;
  }
``` 

For further information, refer to the [Official Nginx Guide](https://nginx.org/en/docs/).

### Login & Connector Settings
The user interface is in a container that was installed on your premisses on the previous step. It can be accessed through the url:
```
http://localhost:8080/
```

1.	You should see the login interface, sign-in using the username & password that you received from the Data Space Connector administrator.

2.	Navigate to the connector settings by the sidebar menu & define the urls of your Local Api Url, Data App Url, Ecc Url. Those 3 connector applications are running on the containers that you installed, so the urls must be configured accordingly as shown below.

#### Local Api Url
The url must be http://your_ip_where_the_containers_are_installed:30001/api

#### Data App Url
In the default testing configuration the given app is exposed to the URL https://be-dataapp-provider:8083 or  https://be-dataapp-consumer:8084.
If you use an external Data app, the Data App must be publicly exposed with a static ip via https, before saved on the connection settings. This happens because Data App is served as an endpoint for peer to peer file transfer between you and other Data Space Connector users. So the url must be the https://your_static_url_that_points_to_dataapp_container.

#### Ecc Url
In the default testing configuration the ECC is exposed to the URL https://ecc-consumer:8889/data or  https://ecc-provider:8889/data.
If you use an external ECC, the url must be publicly exposed with a static ip via https, before saved on the connection settings. This happens because Ecc Url is served as an endpoint for peer to peer file transfer between you and other Data Space Connector users. So the url must be the https://your_static_url_that_points_to_ecc_url_container


### Environment Configuration

Inside the */docker* project folder, there is an *.env* environment configuration file. This file allow you to set all Back End configurations of the Data Space Connector. 

#### Blockchain Notarization Service
The Blockchain Notarization Service is disabled by default. To enable it, use the env variable below.
```
NOTARIZATION_ENABLED = **true|false**
```
The blockchain account provided is intended for testing purposes only. For production or large-scale use, each partner should create their own wallet and accounts.

The blockchain used in this project is Polygon. To create a test credential (with Amoy), you can use Alchemy by following this [step-by-step guide](https://docs.alchemy.com/docs/alchemy-quickstart-guide).
You can request test tokens using this [service link](https://faucet.polygon.technology/).
By completing this [form](https://docs.google.com/forms/d/e/1FAIpQLSe4npoGldJknEs9EBtPaV3AS-0HTso2IuMWDCiMmLEMCx8euQ/viewform), you can receive 100 tokens on the Amoy testnet for free.

To configure the account to be used in notarization service can be used the env variables below.
```
AMOY_API_KEY
MNEMONIC
```

#### Push Mechanism Flow
The Push mechanism flow is enabled by default. To disable the function use the env variable below.
```
PUSH_ENABLED = **true|false**
```
The Push URI can be configured in Push service creation interface.

#### NATS Component
The NATS Component allows, among other things, IEEE 2030.5 integration with the Energy Data Space Framework.
For details about the NATS Component, please refer to the [specific repository](https://github.com/Horizont-Europe-Interstore/Data-Space-Nats-Client).


## Postman collections TRUEConnector

[TRUEConnector_enviroment.postman_environment](TRUEConnector_enviroment.postman_environment.json)

[TRUEConnector.postman_collection](TRUEConnector.postman_collection.json)

## Postman collections Data-App

[be-data-app.postman_collection.json](be-data-app.postman_collection.json)