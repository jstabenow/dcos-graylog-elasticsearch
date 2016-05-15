# DCOS-Graylog-Elasticsearch
A quick howto about how connect a [Graylog-Server](https://github.com/Graylog2/graylog2-server) with the [Elasticsearch-Mesos](https://github.com/mesos/elasticsearch) Framework on [DC/OS](https://dcos.io/).

**Update:**
```sh
$ dcos package repo add universe-jstabenow https://github.com/jstabenow/universe/archive/version-2.x.zip
$ dcos package install elasticsearch
$ dcos package install mongodb
$ dcos package install graylog
````

## Todos
1. Install the Elasticsearch-Mesos Framework
2. Start MongoDB and Graylog

## Install Elasticsearch 
That is easy ;-) Open the "Universe" of the DC/OS-UI and choose "Elasticsearch" or use the DC/OS-CLI:
```sh
$ dcos package install elasticsearch
```

**The default framework settings I am working with:**
* Frameworkname: elasticsearch
* Executorname: elasticsearch-executor
* Clustername: mesos-ha

## Start MongoDB and Graylog
Next steps are requiring the DC/OS CLI:

1. **Clone this repo**
  ```sh
$ git clone https://github.com/jstabenow/dcos-graylog-elasticsearch
$ cd dcos-graylog-elasticsearch/
```

2. **Start MongoDB**
  ```sh
$ dcos marathon app add mongodb.json
```

3. **Start Graylog**

  This requires a running Elasticsearch-Executor and the MongoDB!  

  Important Graylog-Enviroments:
  ```sh
GRAYLOG_ELASTICSEARCH_CLUSTER_NAME=mesos-ha
GRAYLOG_ELASTICSEARCH_DISCOVERY_ZEN_PING_UNICAST_HOSTS=elasticsearch-executor.elasticsearch.mesos:1026
GRAYLOG_MONGODB_URI=mongodb://mongodb.marathon.mesos/graylog
```

  *To verify the port of the "GRAYLOG_ELASTICSEARCH_DISCOVERY_ZEN_PING_UNICAST_HOSTS" env., open the Elasticsearch-Framework-UI and check the "Transport IP" of the tasks page. If Mesos has assigned another port, please also change the value in the "graylog.json"*

  If everything is okay, start Graylog:
  ```sh
$ dcos marathon app add graylog.json
```

  To obtain the address of the Graylog-UI, use
  ```sh
$ dcos marathon task list graylog --json
```

  copy the values of "ipAddress" and the first port of "ports" into you browser e.g. "http://192.168.65.111:28089/".    
  You can pick the address on the Marathon-UI alternativly.

  That's it :-) Hope it works^^

