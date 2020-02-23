# docker-pfelk [![Build Status](https://travis-ci.org/3ilson/docker-pfelk.svg?branch=master)](https://travis-ci.org/3ilson/docker-pfelk)
Deploy pfelk with docker-compose

# Docker Status = "Work in progress"
- [X] Docker Working
- [ ] Create download script
- [X] Create Instructions
- [ ] Create Video Tutorial 

### (1) Required Prerequisits 
- [X] Docker 
- [X] Docker-Compose
- [X] Maxmind 

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
sudo apt-get install geoipupdate
```
### (2) Download pfELK Docker
```
sudo wget https://github.com/3ilson/docker-pfelk/archive/master.zip
```
#### (2a) Unzip pfelk.zip
```
sudo apt-get install unzip
```
```
sudo unzip master.zip
```
#### (2b) Enter License Key
- Ceate a Max Mind Account @ https://www.maxmind.com/en/geolite2/signup
- Login to your Max Mind Account; navigate to "My License Key" under "Services" and Generate new license key
- Enter the Account ID and Key to the file below
```
sudo nano /etc/GeoIP.conf
```
- Wait up to 5min and initiate geoipupdate
```
geoipupdate
```
#### (2c) Copy GeoIP Files (pfelk.zip:/logstash/GeoIP)
- [X] GeoLike2-ASN.mmdb 
- [X] GeoLite2-City.mmdb
- [X] GeoLite2-Country.mmdb 
```
sudo cp /usr/share/GeoIP/GeoLite2-City.mmdb docker-pfelk-master/logstash/GeoIP/
sudo cp /usr/share/GeoIP/GeoLite2-Country.mmdb docker-pfelk-master/logstash/GeoIP/
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
### (4) Configuration
#### (4a) Edit 01-inputs.conf (pfelk.zip:/logstash/pipeline/01-inputs.conf)
Amend line #9 to match your pfSense or OPNsense IP address
#### (4b) Edit 01-inputs.conf (pfelk.zip:/logstash/pipeline/01-inputs.conf)
Amend line 24-29 comment or uncomment the OPNsense or pfSense grok pattern
### (5) Start Docker 
```
sudo docker-compose up
```
Once fully running, navigate to the host ip (ex: 192.168.0.100:5601)
