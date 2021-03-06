
# Demo

## Clone the repo and switch to the checked out repo directory:

```
git clone https://github.com/auramirea/cloudfoundry-service-broker.git
cd cloudfoundry-service-broker
```

## Pre-requisites
1. Ensure gradle is installed
2. Ensure PCF dev is installed abd running

## Running the scripts individually:
```
./cf-setup.sh
cd service && ./service-deploy.sh && cd ..
cd broker && ./broker-deploy.sh && cd ..
cd client && ./client-deploy.sh && cd ..
```

## Tear down script

```./down.sh```

## Running the fully automated script that deploys all three apps:
```
./up.sh
```

# Overview

Simple service broker that provides a virusscanner service with only one plan (free). 
The virusscanner tests if a file contains a virus by checking the suffix of the filename. If the filename ends in '.virus' then 
the file contains a virus, otherwise not.
To demo the service broker, a test application (demoFileUploader) is used. 

## Deploy the Service Broker to Cloud Foundry
Build it and push it:
```
./gradlew build & cf push
```

Register the service broker using the default username and the password obtained from the previous step:
```
cf create-service-broker generic-service-broker admin admin http://generic-service-broker.local.pcfdev.io
```

Check the list of service brokers:
```
cf service-brokers
```

Enable access to the service broker offering for all plans:
```
cf enable-service-access virusscanner
```

Check that your service is in the marketplace:
```
cf marketplace
```

## Push the virusscanner application
Build and push it:
```
./gradlew build & cf push
```

Create service instance:
`cf create-service virusscanner free free-virusscanner`

## Push the demo application
Build and push it:
```
./gradlew build & cf push
```

Call the app URL and upload a file to test if it contains a virus or not.
