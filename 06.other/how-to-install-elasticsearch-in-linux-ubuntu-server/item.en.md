---
title: 'How to install Elasticsearch in Linux Ubuntu server '
media_order: logo-elastic.png
date: '22-02-2016 16:22'
taxonomy:
    tag:
        - Linux
        - Elasticsearch
header_image_file: logo-elastic.png
---

I am currently working on three computers and it is real pain to get everything synchronized. Of course I am using git repositories and cloud but I still have to get rid of apache server, databases, etc. So I decided to create Virtual Machine which I save on USB drive and try to do it this way.

===

The Linux installation with Apache and MySql is not a problem. But I needed to install Elasticsearch ( which runs on Java ) and make it accessible from my computer ( Linux with Elasticsearch is running in VM ).

## Installing Java 8

#### Add Java repository

Download the required files from Oracle :

```
sudo add-apt-repository -y ppa:webupd8team/java
```

Update :

```
sudo apt-get update
```

#### Install Java

```
sudo apt-get install oracle-java8-installer
```

Check ( verify ) version after installation :

```
java -version
```

On the output you should see something like this :

```
java version “1.8.0_72”
Java(TM) SE Runtime Environment (build 1.8.0_72-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.72-b15, mixed mode)
```

## Installing ElasticSearch

#### Add elastic repository

```
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

#### Create repository list for Elastic

```
echo "deb http://packages.elastic.co/elasticsearch/1.7/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-1.7.list
```

Update again :

```
sudo apt-get update
```

#### Install Elastic

```
sudo apt-get install elasticsearch
```

#### Enable Elastic to start automatically

```
sudo update-rc.d elasticsearch defaults 95 10
```

Sample output :

```
Adding system startup for /etc/init.d/elasticsearch …
/etc/rc0.d/K10elasticsearch -> ../init.d/elasticsearch
/etc/rc1.d/K10elasticsearch -> ../init.d/elasticsearch
/etc/rc6.d/K10elasticsearch -> ../init.d/elasticsearch
/etc/rc2.d/S95elasticsearch -> ../init.d/elasticsearch
/etc/rc3.d/S95elasticsearch -> ../init.d/elasticsearch
/etc/rc4.d/S95elasticsearch -> ../init.d/elasticsearch
/etc/rc5.d/S95elasticsearch -> ../init.d/elasticsearch
```

## Configuration

#### Change network host and allow remote connection

Open Elasticsearch configuration file in your favorite editor, allow and change `network.host` to `localhost`. And if you want to connect to Elasticsearch from remote machine you need to enable `network.bind_host` and set it to `0`.

```
sudo vi /etc/elasticsearch/elasticsearch.yml
```

Part of my Elasticsearch configuration file :

```
############################## Network And HTTP ###############################
# Elasticsearch, by default, binds itself to the 0.0.0.0 address, and listens
# on port [9200-9300] for HTTP traffic and on port [9300-9400] for node-to-node
# communication. (the range means that if the port is busy, it will automatically
# try the next port).

# Set the bind address specifically (IPv4 or IPv6):
#
network.bind_host: 0

# Set the address other nodes will use to communicate with this node. If not
# set, it is automatically derived. It must point to an actual IP address.
#
#network.publish_host: 192.168.0.1

# Set both ‘bind_host’ and ‘publish_host’:
#
network.host: localhost
```

#### Restart Elasticsearch service

```
sudo /etc/init.d/elasticsearch restart
```

## Conclusion

Now, you should be able to connect and get data from Elasticsearch. Test it with command :

```
curl -X GET 'http://localhost:9200'
```

Response :

```
{
  “status” : 200,
  “name” : “Diablo”,
  “cluster_name” : “elasticsearch”,
  “version” : {
    “number” : “1.7.5”,
    “build_hash” : “00f95f4ffca6de89d68b7ccaf80d148f1f70e4d4”,
    “build_timestamp” : “2016-02-02T09:55:30Z”,
    “build_snapshot” : false,
    “lucene_version” : “4.10.4”
  },
  “tagline” : “You Know, for Search”
}
```