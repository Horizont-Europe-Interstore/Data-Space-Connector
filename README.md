# Energy-Data-Space-Connector

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

3.	Start the containers with the below commands:
```
docker compose up -d --build
```

4.	To show logs use the command:
```
docker-compose logs -f
```
Alternatively you can use dozzle UI to access the logs of each container. Open the following url on your browser :
```
http://localhost:8081
```

5.	If no errors are seen, this means that Energy Data Space Connector was successfully deployed on your premisses.

### Login & Connector Settings
The user interface is in a container that was installed on your premisses on the previous step. It can be accessed through the url:
```
http://localhost:8080/
```

1.	You should see the login interface, sign-in using the username & password that you received from the Interstore Connector administrator.

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
The Blockchain Notarization Service is disabled by default. To enable the service use the env variable below.
```
NOTARIZATION_ENABLED = **true|false**
```


## Postman collections TRUEConnector

[TRUEConnector_enviroment.postman_environment](TRUEConnector_enviroment.postman_environment.json)

[TRUEConnector.postman_collection](TRUEConnector.postman_collection.json)

## Postman collections Data-App

[be-data-app.postman_collection.json](be-data-app.postman_collection.json)