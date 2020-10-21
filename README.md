# docker-pfelk 
Deploy pfelk with docker-compose [Video Tutorial](https://www.youtube.com/watch?v=xl0v9h8RXBc) 

![Version badge](https://img.shields.io/badge/ELK-7.9.2-blue.svg)
[![Build Status](https://travis-ci.org/3ilson/docker-pfelk.svg?branch=master)](https://travis-ci.org/3ilson/docker-pfelk)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/a3ilson) 

### (1) Required Prerequisits 
- [X] Docker 
- [X] Docker-Compose
- [X] Maxmind 
- [X] Java 

#### (1a) Docker Install
```
sudo apt-get install docker
```
```
sudo apt-get install docker-compose
```
#### (1b) MaxMind Install
```
sudo apt-get install software-properties-common
```
```
sudo add-apt-repository ppa:maxmind/ppa
```
```
sudo apt install geoipupdate
```
### (2) Download pfELK Docker
```
sudo wget https://github.com/3ilson/docker-pfelk/raw/master/pfelkdocker.zip
```
#### (2a) Unzip pfelkdocker.zip
```
sudo apt-get install unzip
```
```
sudo unzip pfelkdocker.zip
```
#### (2b) Enter License Key
- Ceate a Max Mind Account @ https://www.maxmind.com/en/geolite2/signup
- Login to your Max Mind Account; navigate to "My License Key" under "Services" and Generate new license key
- Enter the Account ID and Key to the file below (lines 7 & 8)
```
sudo nano /etc/GeoIP.conf
```
- Modify line 13 as follows:
```
EditionIDs GeoLite2-City GeoLite2-Country GeoLite2-ASN
```
- Wait up to 5min and initiate geoipupdate
```
sudo geoipupdate -d /usr/share/GeoIP/
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
Replace docker-compose.yml with this version of [docker-compose.yml](https://raw.githubusercontent.com/3ilson/docker-pfelk/master/scale/docker-compose.yml)

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

Finalize templates and dashboards [here](https://github.com/3ilson/docker-pfelk/blob/master/finalize.md)
