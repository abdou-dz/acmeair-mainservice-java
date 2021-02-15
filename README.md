
## Acme Air Main Service - Java/Liberty

An implementation of the Acme Air Main Service for Java/Liberty. The main service primarily consists of the presentation layer (web pages) that interact with the other services.

This implementation can support running on a variety of runtime platforms including standalone bare metal system, Virtual Machines, docker containers, IBM Cloud, IBM Cloud Private, and other Kubernetes environments.

## Full Benchmark Installation Instructions on various docker enviornments.
![alt text](https://github.com/yigitpolat/acmeair-mainservice-java/blob/master/images/AcmeairMS.png "AcmeairMS Java")

## Installing Acme Air on Openshift 4.x

### Method 1: Build & Deploy
Podman Environment
```
$ yum install podman maven git -y

$ mkdir acme-air
$ cd acme-air
$ git clone https://github.com/abdou-dz/acmeair-mainservice-java.git
$ git clone https://github.com/abdou-dz/acmeair-authservice-java.git
$ git clone https://github.com/abdou-dz/acmeair-customerservice-java.git
$ git clone https://github.com/abdou-dz/acmeair-flightservice-java.git
$ git clone https://github.com/abdou-dzt/acmeair-bookingservice-java.git

$ cd acmeair-mainservice-java
$ mvn clean package
$ podman build -t docker.io/yigitpolat/acmeair-mainservice-java:$(uname -m) --format docker -f Dockerfile .
$ podman push docker.io/yigitpolat/acmeair-mainservice-java:$(uname -m)

$ cd ../acmeair-authservice-java
$ mvn clean package
$ podman build -t docker.io/yigitpolat/acmeair-authservice-java:$(uname -m) --format docker -f Dockerfile .
$ podman push docker.io/yigitpolat/acmeair-authservice-java:$(uname -m)

$ cd ../acmeair-customerservice-java
$ mvn clean package
$ podman build -t docker.io/yigitpolat/acmeair-customerservice-java:$(uname -m) --format docker -f Dockerfile .
$ podman push docker.io/yigitpolat/acmeair-customerservice-java:$(uname -m)

$ cd ../acmeair-flightservice-java
$ mvn clean package
$ podman build -t docker.io/yigitpolat/acmeair-flightservice-java:$(uname -m) --format docker -f Dockerfile .
$ podman push docker.io/yigitpolat/acmeair-flightservice-java:$(uname -m)

$ cd ../acmeair-bookingservice-java
$ mvn clean package
$ podman build -t docker.io/yigitpolat/acmeair-bookingservice-java:$(uname -m) --format docker -f Dockerfile .
$ podman push docker.io/yigitpolat/acmeair-bookingservice-java:$(uname -m)

$ oc login -u <username> -p <password> <api-endpoint>

$ cd ../acmeair-mainservice-java/scripts
$ sh deployToOpenShift.sh

$ curl --insecure https://acmeair.apps.test.cluster.tr.com:443/booking/loader/load
$ curl --insecure https://acmeair.apps.test.cluster.tr.com:443/flight/loader/load
$ curl --insecure https://acmeair.apps.test.cluster.tr.com:443/customer/loader/load?numCustomers=10000
```

### Method 2: Deploy using OpenShift templates via CLI
```
$ git clone https://github.com/yigitpolat/acmeair-mainservice-java.git
$ oc login -u <username> -p <password> <api-endpoint>
$ cd acmeair-mainservice-java/templates
# ephemeral template
# change domain parameter if you are using your own cluster
$ oc new-app -f acme-air-persistent-ephemeral.yaml
# persistent template
# change domain parameter if you are using your own cluster
$ oc new-app -f acme-air-persistent-template.yaml
```

### Method 3: Deploy using OpenShift templates via OpenShift GUI
```
$ git clone https://github.com/yigitpolat/acmeair-mainservice-java.git
# ephemeral template
$ oc create -f acme-air-persistent-ephemeral.yaml
# persistent template
$ oc create -f acme-air-persistent-template.yaml
```

- Navigate to OpenShift GUI > Developer > +Add > From Catalog

![alt text](https://github.com/yigitpolat/acmeair-mainservice-java/blob/master/images/catalog.png "Catalog")
