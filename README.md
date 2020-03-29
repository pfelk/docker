![pfelk dashboard](https://github.com/3ilson/docker-pfelk/blob/master/pfelk%2Bdocker.jpg)
# docker-pfelk [![Build Status](https://travis-ci.org/3ilson/docker-pfelk.svg?branch=master)](https://travis-ci.org/3ilson/docker-pfelk)
Deploy pfelk with docker-compose [Video Tutorial](https://www.youtube.com/watch?v=xl0v9h8RXBc) 

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
sudo wget https://github.com/3ilson/docker-pfelk/raw/master/pfelkdocker.zip
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
sudo geoipupdate
```
#### (2c) Copy GeoIP Files (pfelk.zip:/logstash/GeoIP)
```
sudo cp /usr/share/GeoIP/GeoLite2-ASN.mmdb /ENTERYOURPATH/logstash/GeoIP/
sudo cp /usr/share/GeoIP/GeoLite2-City.mmdb /ENTERYOURPATH/logstash/GeoIP/
sudo cp /usr/share/GeoIP/GeoLite2-Country.mmdb /ENTERYOURPATH/logstash/GeoIP/
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
```
Change line 9; the "if [host] =~ ..." should point to your pfSense/OPNsense IP address
Change line 12; rename "firewall" (OPTIONAL) to identify your device (i.e. backup_firewall)
Change line 15-22; (OPTIONAL) to point to your second PF IP address or ignore
```
#### (4b) Edit 01-inputs.conf (pfelk.zip:/logstash/pipeline/01-inputs.conf)
```
For pfSense uncommit line 30 and commit out line 27
For OPNsense uncommit line 27 and commit out line 30
```
### (5) Start Docker 
```
sudo docker-compose up
```
Once fully running, navigate to the host ip (ex: 192.168.0.100:5601)
