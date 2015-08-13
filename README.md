## realtime analytics using logstash
Data Source ( NodeA , NodeB , Node C)
  =>
Logforwarder / Kafka (client -> Q)
  =>
Logstash Server
  =>
(parse, filter, query) 
  =>
email, http, statsD , nodejs-rules engine , FlabJack
  =>
persist final result into ES => Kibana , Custom Dashboards 

========================

## How to setup logstash-forwarder in a log-shipping node ?

### 1. download logstash-forwarder 
#### setup logstash-forwarder in every physical node 
#### mv logstash-forwarder /opt/
### 2. download the certificates from this repo
### 3. copy the certificates in respective folders
#### cp logstash-forwarder.crt /etc/ssl/certs/
#### cp logstash-forwarder.key /etc/ssl/private/
### 4. specify input file location in config file
#### cp logstash-forwarder.config /etc/
#### change the file path as per the new node's logfile location and specify a tag.
### 5. start the logstash-forwarder 
####  cd /opt/logstash-forwarder; sudo nohup ./bin/logstash-forwarder -config /etc/logstash-forwarder.conf -from-beginning=false &
   
** Note -> we need to update logstash-forwarder.config to dump logs in syslog file , otherwise all log will be stored in nohup.out

## How to setup logstash in log-analyzer node ?
### 1. the sample logstash.conf can be downloaded from git
#### the configuration can be updated with data sources,  filters, outputs ( http / statsD / email ) for specific type of source.
### 2. the certficate and security files need to be copied
### 3. start the logstash-agent
#### cd /app/logstash/; ./logstash-executor

## How to leverage logstash to send log events ?
#### we can start logstash-forwarder in box1 
##### cd /opt/logstash-forwarder; sudo ./bin/logstash-forwarder -config /etc/logstash-forwarder.conf -from-beginning=false
#### we can start logstash-agent in box2
##### simply run the following command
##### cd /app/logstash/; ./logstash-executor
##### we will receive email notification as soon as an OutOfMemoryError occurs in the box1
