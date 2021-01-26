# Couchbase Mobile Sync Gateway Postman Collection
These are the Postman Collections and environment variable definition files for REST Interface to Couchbase Mobile Sync Gateway (Couchbase Mobile 1.5).

#### Sync-Gateway-Admin.postman_collection 

[This](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-Admin.postman_collection) is the Postman collection corresponding to the [Admin interface](https://docs.couchbase.com/sync-gateway/current/refer/rest-api-admin.html) of the Sync Gateway

####  Sync-Gateway-Public.postman_collection

[This](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-Public.postman_collection) is the Postman collection corresponding to the [Public interface](https://docs.couchbase.com/sync-gateway/current/refer/rest-api-public.html) of the Sync Gateway

####  Sync-Gateway-Environment.postman_environment
[This](https://raw.githubusercontent.com/couchbaselabs/Couchbase-Sync-Gateway-Postman-Collection/master/Sync-Gateway-Public.postman_collection) is the Environment Definitions file that defines the variables used by the Admin and Public collections

####  sync-gateway-config.json
This is very simple sample sync gateway file corresponding to the sample environment that you can use to jumpstart your testing. If you have a sync gateway running, you can relaunch your sync gateway with this sample config file.

- By default, the collections point to a mock server
- Update the "adminurl" and "publicurl" environment variables with the Admin and Public URL Sync Gateway endpoints of your setup .


### Prequiresites
You should have Sync Gateway [installed](https://www.couchbase.com/downloads) and running. 

### Usage
- If you haven't already done so, download [Postman](https://www.getpostman.com/) for free for your platform. There is a Pro version as well if you want the added functionality.
- Launch Postman and import the collections and environment files. Update the environment variables to correspond to your setup.


If you are new to Postman, read more on how you can get started with this collection in this [blog post](https://blog.couchbase.com/querying-couchbase-sync-gateway-with-postman/).

![Alt text](http://blog.couchbase.com/wp-content/uploads/2017/04/postman_featured-e1492094985530.png "Using Postman to Query The Sync Gateway Web Interface")
