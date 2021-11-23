# Overview

Couchbase Sync Gateway supports a [RESTful interface](https://docs.couchbase.com/sync-gateway/3.0/rest-api-access.html) that allows web based clients to administer, monitor and interact with the Sync Gateway. With the 3.0 release, Sync Gateway database admistration is handled exclusively via the admin REST endpoint. 

This repo contains [Postman](https://learning.postman.com/docs/getting-started/introduction/) Collections and environment variable definition file corresponding to **v3.0** of Sync Gateway REST Interface. Think of a Postman Collection as a group of saved requests, organized in folders and subfolders. You can use the collections as a starting point and customize it per your environment. Then, use the [Postman API client](https://www.postman.com/product/api-client/) to send reqests to the Sync Gateway and to inspect responses. Only RBAC users with the right roles and privileges can excute the requests.

In addition, the repo also contains a `TravelSample` folder that contains a custom collection for a Travel Sample application. More details on that follow.

***Disclaimer***: The Postman collections in this repo are shared as a convenience for administrators and developers to get started with Sync Gateway. It is not officially supported by Couchbase and as such, there are no guarantees that the Collections will remain up-to-date with the evolution of the REST API. To that end, contributions to keep the collections up-to-date are welcome.

# Compatibility
The collection supports v3.0 of Sync Gateway REST interface.

# Postman Collections

### Sync-Gateway-3.0-Admin-API.postman_collection.json

[Sync-Gateway-3.0-Admin-API.postman_collection](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-3.0-Admin-API.postman_collection.json) corresponds to the [Sync Gateway Admin interface](https://docs.couchbase.com/sync-gateway/3.0/rest-api-admin.html) that includes database management and administration, defining access controls and end user management.


### Sync-Gateway-3.0-Public-API.postman_collection.json

[Sync-Gateway-3.0-Public-API.postman_collection](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-3.0-Public-API.postman_collection.json) corresponds to the [Sync Gateway Public interface](https://docs.couchbase.com/sync-gateway/3.0/rest-api.html) that enables read/write access to application data.

### Sync-Gateway-3.0-Metrics-API.postman_collection.json

[Sync-Gateway-3.0-Metrics-API.postman_collection](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-3.0-Metrics-API.postman_collection.json) corresponds to the [Sync Gateway Metrics interface](https://docs.couchbase.com/sync-gateway/3.0/rest-api.html) that enables access to cluster-level stats in Prometheus and custom JSON format.


##  Sync-Gateway-3.0-Environment.postman_environment
[Sync-Gateway-3.0-Environment.postman_environment](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-3.0-Environment.postman_environment.json) is the Environment Definitions file that defines the variables. The variables will have to be customized with values corresponding to your environment.

## Usage

*  Download [Postman client](https://www.getpostman.com/) for free for your platform. There is a Pro version as well if you want the added functionality.

* Launch Postman API client and [import](https://learning.postman.com/docs/running-collections/working-with-data-files/) the collections and environment file

* Update the environment file settings per your environment and start executing requests against your Sync Gateway

# Demonstration using Travel Sample Postman Collections 

We have put together a custom version of the Postman collections corresponding to Travel Sample Application. These are located in the "travel-sample" folder of the repo. A few of the requests in the collections also include some simple tests to verify if the request was succesful or not.

## Pre-requisites

*  Download [Postman client](https://www.getpostman.com/) for free for your platform. There is a Pro version as well if you want the added functionality.

* [Docker](https://docs.docker.com/get-docker/) must be installed on your laptop. On Windows, you may need admin privileges. Ensure that you have sufficient memory and cores allocated to docker. At Least 3GB of RAM is recommended.

## Collections

* _Sync-Gateway-3.0-TravelSample-Admin-API.postman_collection.json_: Collection of requests to create Sync Gateway travel-sample database, to configure and to manage the database and to create Sync Gateway users. All requests in this collection have to be authenticated .

* _Sync-Gateway-3.0-TravelSample-Public-API.postman_collection.json_: Collection of requests to create, update and access documents from travel-sample database. All requests in this collection have to be authenticated.

* _Sync-Gateway-3.0-TravelSample-Metrics-API.postman_collection.json_: Collection of requests to create, update and access documents from travel-sample database. All requests in this collection have to be authenticated.

## Getting Started

To be able to run the collection, you need an enviroment with Couchbase Server and Sync Gateway. Running it in docker containers is the simplest option. We have pre-built images that has everything configured and good to go.

### Docker Network

* Create a docker network named “workshop”

```bash
docker network ls 

docker network create -d bridge workshop
```

### Install Couchbase Server

We have a custom docker image`priyacouch/couchbase-server-travelsample:7.x-dev` of  couchbase server. This image
(a) Loads the travel-sample bucket
(b) Configures three RBAC users - 
    - "admin" user is the user credentials with which the Sync Gateway connects to Couchbase Server
    - "sgw-cluster" user which is used for cluster-level operations via the Sync Gateway admin/metrics REST endpoint
    - "sgw-admin" user which is used for sync gateway database-level administrative operations via the Sync Gateway admin/metrics REST endpoint

```bash
docker stop cb-server

docker rm cb-server

docker run -d --name cb-server --network workshop -p 8091-8094:8091-8094 -p 11210:11210 priyacouch/couchbase-server-travelsample:7.x-dev

```

#### Test Installation

* The server would take a few minutes to deploy and get fully initialized. So be patient. 

```bash
docker logs -f cb-server
```

* You should see the following once setup is completed

```bash

100    50    0     0  100    50      0   4166 --:--:-- --:--:-- --:--:--  4545
* Closing connection 0
SUCCESS: Bucket created
SUCCESS: User admin set
/entrypoint.sh couchbase-server
```

* Then open up http://localhost:8091 in browser
Sign in as “Administrator” and “password” in login page

* Go to “buckets” menu and confirm that the `travel-sample` bucket is loaded. This bucket contains sample data.
![](https://blog.couchbase.com/wp-content/uploads/2021/11/admin-ui-bucket.png)

* Go to “security” menu and confirm the following users are created. The users have been explained earlier.
![](https://blog.couchbase.com/wp-content/uploads/2021/11/admin-ui-security.png)

### Install Sync Gateway

* Sync Gateway needs to be bootstrapped with a startup configuration file. The [sync-gateway-config-travelsample-bootstrap.json]() file is available in the sample "travel-sample" folder. 

* Download the configuration file. Switch to the the folder which contains the downloaded json file and run the following commands

```bash
cd /path/to/cloned/repo/travel-sample
```

* Follow these instructions to deploy Sync Gateway with the downloaded config file

```bash
docker stop sync-gateway

docker rm sync-gateway
```

  * **Windows Systems**:

```bash
docker run -p 4984-4986:4984-4986 --network workshop --name sync-gateway -d -v %cd%/ync-gateway-config-travelsample-bootstrap.json:/etc/sync_gateway/sync_gateway.json couchbase/sync-gateway:3.0.0-beta02-enterprise /etc/sync_gateway/sync_gateway.json
```

  * **Non-Windows Systems**:

```bash
docker run -p 4984-4986:4984-4986 --network workshop --name sync-gateway -d -v `pwd`/ync-gateway-config-travelsample-bootstrap.json:/etc/sync_gateway/sync_gateway.json couchbase/sync-gateway:3.0.0-beta02-enterprise  /etc/sync_gateway/sync_gateway.json
```


#### Test Installation

* You can confirm that the Sync Gateway is up and running by verifying the log messages

```bash
docker logs -f sync-gateway
```

You will see bunch of log messages. Make sure no errors

* Then  open up http://localhost:4984 in browser
You should see equivalent of the following message

```bash
{"couchdb":"Welcome","vendor":{"name":"Couchbase Sync Gateway","version":"3.0"},"version":"Couchbase Sync Gateway/{version-maintenance}(145;e3f46be) EE"}
```

### Using Postman

* Launch Postman API client and import all the three "travelsample" collections and the corresponding `Sync-Gateway-3.0-TravelSample-Environment.postman_environment` environment file. The environmental file has been customized for the travel-sample applicatin. Spend some time exploring the same.

* All the requests are setup to use the relevant user for authorization. However, you will need to provide password. All the users are setup with a password of "password". So make sure you configure the "Authorization header" of your requests.

![](https://blog.couchbase.com/wp-content/uploads/2021/11/set-auth-header.png)


#### Sanity Test

* First, lets do a sanity test. For that run the "Get Sync Gateway Info" request from the Admin API collection. You should see details of the Sync Gateway in the response.

![](https://blog.couchbase.com/wp-content/uploads/2021/11/get-sync-gateway-info.png)

#### Create & Configure Travel Sample Database

Depending on whether the request is a cluster-level request or a database-level request, the user specified in the Authorization header may be "{{clusteradmin}}" or "{{dbadmin}}" respectively. Use password of "password" for requests.

* Starting v3.0, all sync gateway database administration including database creation is handled via the admin REST endpoint. So this means that you would need to first create a sync gateway database.
For this run the "Create a Sync Gateway Database" request from the "Sync Gateway 3.0 TravelSample Admin API" collection as shown below. Once database is created, you can run other operations on the database like creating a sync function, setting import filters etc. 

![](https://blog.couchbase.com/wp-content/uploads/2021/11/db-creation.gif#800x490)

#### Creating a Sync Gateway user

* Before you can start interacting with the Sync Gateway database via the public REST endpoint, you will have to create a [sync gateway user](https://docs.couchbase.com/sync-gateway/3.0/users.html). For this run the "Create a new user" request from the "Sync Gateway 3.0 TravelSample Admin API" collection as shown below. This will create a user named "demo" with password of "password". Once user is created, you can use the public REST endpoint to interact with the database.

![](https://blog.couchbase.com/wp-content/uploads/2021/11/users.gif#800x490)

#### Data Operations

Accessing documents in the database and modifying it can be done via the either the admin REST API or the public REST API. In this example, we will use the pubic interface. All requests are executed using "{{username}}" user that was created via the "Create a new user" request discussed earlier. Use password of "password" for requests.

* The travel-sample bucket is preloaded with sample data. You can retrieve that using the "Get Document" request from the "Sync Gateway 3.0 TravelSample Public API" collection. Replace "newdoc" with "doc" in the request to retrieve an existing document from bucket. 

* You can also create a new document and edit it by running requests from the "Sync Gateway 3.0 TravelSample Public API" collection as shown below.

![](https://blog.couchbase.com/wp-content/uploads/2021/11/documents.gif#800x490)

#### Monitoring Operations

* Finally, you can monitor the Sync Gateway by running the "Debugging/monitoring runtime stats in Prometheus format" request from the "Sync Gateway 3.0 TravelSample Metrics API" as shown below.

![](https://blog.couchbase.com/wp-content/uploads/2021/11/monitor.gif#800x490)

## Next Steps
Familiarize yourself with the other REST APIs in the travel-sample collections by running through the other requests and customizing the requests as needed.Bug fix and enhancement contributions welcome. 


