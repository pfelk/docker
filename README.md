# docker-pfelk 
Deploy pfelk with docker-compose [Video Tutorial](https://www.youtube.com/watch?v=xl0v9h8RXBc) 

![Version badge](https://img.shields.io/badge/ELK-7.10.1-blue.svg)
[![Build Status](https://travis-ci.org/pfelk/docker.svg?branch=master)](https://travis-ci.org/pfelk/docker-pfelk)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/a3ilson) 

### (1) Required Prerequisits 
- [X] Docker 
- [X] Docker-Compose

#### (1a) Docker Install
```
sudo apt-get install docker
```
```
sudo apt-get install docker-compose
```
#### (1b) MaxMind Install (Optional) 
Follow the steps [here](https://github.com/pfelk/pfelk/wiki/How-To:-MaxMind-via-GeoIP-with-pfELK), to install and utilize MaxMind. Otherwise the built-in GeoIP from Elastic will be utilized.

### (2) Download pfELK Docker
```
sudo wget https://github.com/pfelk/docker-pfelk/raw/master/pfelkdocker.zip
```
#### (2a) Unzip pfelkdocker.zip
```
sudo apt-get install unzip
```
```
sudo unzip pfelkdocker.zip
```
### (3) Memory 
#### (3a) Set vm.max_map_count to no less than 262144 (must run each time host is booted)
```
sudo sysctl -w vm.max_map_count=262144
```
#### (3b) Set vm.max_map_count to no less than 262144 (one time configuration) 
```
sudo echo "vm.max_map_count=262144" >> /etc/sysctl.conf
```
### (4) Start Docker 
```
sudo docker-compose up
```
Once fully running, navigate to the host ip (ex: 192.168.0.100:5601)

### Scaling out pfelk
Replace docker-compose.yml with this version of [docker-compose.yml](https://raw.githubusercontent.com/pfelk/docker/master/scale/docker-compose.yml)

#### (0) Prerequisites

Please visit the [following documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) for additional details.

**Randomize published ports**

Use either `--publish-all` or enable random ports for hosts, for example:

```
  elasticsearch:
    ports:
      - '9200'
```

**Enable the data path to be shared by multiple nodes**

For example, if you want to scale out to 3 nodes, use the following value:

```
  elasticsearch:
    environment:
      node.max_local_storage_nodes: '3'
```

#### (1) Scale out pfelk

Scale out your deployment to 3 nodes by running the following command:

```
sudo docker-compose up -d --scale pfelk=3
```

### (4) Finalizing 

Finalize templates and dashboards [here](https://github.com/pfelk/docker/blob/master/finalize.md)
