# Cloud Carbon Footprint Evaluation

## Table of Contents

- [Introduction](#Introduction)
- [Key findings of evaluation](#key-findings-of-evaluation)
- [Setup and steps used for testing/deploying in a virtual machine](#setup-and-steps-used-for-testingdeploying-in-a-virtual-machine)
- [Setup and deployment to your kubernetes cluster](#setup-and-deployment-to-your-kubernetes-cluster)

# Introduction

Cloud Carbon Footprint is an open-source tool designed for monitoring the environmental impact of cloud infrastructure.
This versatile tool seamlessly supports Azure, AWS, GCP, and AliCloud out of the box.
Moreover, it enhances precision by integrating with the Electricity Maps' API to provide more accurate emissions' data.


## Key findings of evaluation

The tool offers a straightforward setup process that delivers substantial value.
Despite a few display bugs within the interface, it's important to note that these issues do not compromise the accuracy or integrity of the presented data.


## Setup and steps used for testing/deploying in a virtual machine

This setup uses an Azure VM running ubuntu 22.04 to host the Cloud Carbon Footprint app.
There are several methods to run CCF. In this case we're running it with Docker (https://www.cloudcarbonfootprint.org/docs/run-with-docker).
Once the VM is up and running, login and follow the instructions below to get CCF to run.


##### Install docker:

Install docker according to docker's instructions: https://docs.docker.com/engine/install/

##### Clone Cloud Carbon Footprint Repository
The docker containers still uses some configuration files available at the CCF repository, cloning the repository can be helpful.
``` bash
git clone https://github.com/cloud-carbon-footprint/cloud-carbon-footprint.git
```

go to the repository home directory:
``` bash
cd cloud-carbon-footprint
```
##### Prepare your environment:

You can use the file under `packages/api/.env.template` to prepare your own `.env` file.
``` bash
cp ~/cloud-carbon-footprint/packages/api/.env.template ~/cloud-carbon-footprint/packages/api/.env
```

> This file has several inline-comments that return syntax errors if not removed.
> Make sure to remove such comments before running the API container.


##### Connect to Azure:

Follow the guide (https://www.cloudcarbonfootprint.org/docs/azure/)

> A couple of comments on this guide:
>
> App Registrations is part of Entra ID, so only accounts that have access to Entra ID can perform steps 1 and 2.
>
>Step 3 is a bit ambiguous, read rights is enough for the application.
>The built-in "Reader" role is a good alternative for a quick setup.

Once the Azure App is set up, use the correct values on the `.env` file.
```
AZURE_CLIENT_ID=[Your Azure app id]
AZURE_CLIENT_SECRET=[The Azure app secret]
AZURE_TENANT_ID=[Tenant id]
```

You might also want to modify the line:
```
# List of Azure subscriptions to include in estimations (all subscriptions are fetched by default)
AZURE_SUBSCRIPTIONS=["subscription-1", "subscription-2"]
```

##### Data Persistence

The application should be connected to a MongoDB database, however it offers the alternative of local storage.

##### Using Local Storage

By default, the `.env.template` file has several MongoDB settings that will make the api container to fail if not set properly.

If local cache is enough just comment out the lines:
```
CACHE_MODE=MONGODB

MONGODB_URI=mongodb://localhost:27017
MONGODB_CREDENTIALS=/keys/mongodb-certificate.pem
```
##### Using MongoDB (User authentication)

Install MongoDB:
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/

```
# start and enable mongodb
sudo systemctl start mongod
sudo systemctl enable mongod
```
Enter the mongodb shell

```
mongosh
```
Now create a user for CCF to access the database:
```
# switch to the admin database
use admin

# create user
db.createUser( {
    user: "ccf-user",
    pwd: "<apassword>",
    roles: [ "readWrite" ]
})

```
Adjust the settings in your `.env` file.

```
CACHE_MODE=MONGODB
# mongodb://<user>:<password>:<port>/<database>?<authSource>
MONGODB_URI=mongodb://ccf-user:<apassword>:27017/admin?authSource=admin
```

At this point MongoDB will not be listening to the ports used by the docker images.
You can bind MongoDB to listen to available network interfaces in the file `etc/mongo.conf`.

```
#under net
net:
    # modify the bind ip
    bindIp: 0.0.0.0
```

CCF should now be able to connect to the database.

##### Start the Docker containers

CCF guide: https://www.cloudcarbonfootprint.org/docs/run-with-docker

The guide fails to mention that the containers need to be in the same docker network so that the client can connect to the API.

Create a docker network
```
docker network create mynetwork
```

Run the API Docker container
```
docker pull cloudcarbonfootprint/api

docker run \
    --network mynetwork \
    --env-file packages/api/.env \
    -p 4000:4000 \
    cloudcarbonfootprint/api
```
Run the client Docker container
```
docker pull cloudcarbonfootprint/client:latest

docker run \
    --network mynetwork \
    -p 80:80 \
    -v ${PWD}/docker/nginx.conf:/etc/nginx/nginx.conf \
    cloudcarbonfootprint/client:latest
```

## Setup and deployment to your kubernetes cluster

Since for the long term users will probably want to deploy their CCF instance to a k8s cluster, we have a sample project 
with an extensive explanation of what to do in its READ.me https://gitlab.maibornwolff.de/ivory-space/green-in-it/ccf-deployment
