---
title: 'Ako nainštalovať Elasticsearch na Linux Ubuntu serveri'
media_order: logo-elastic.png
published: true
date: '22-02-2016 16:22'
taxonomy:
    tag:
        - Linux
        - Elasticsearch
slug: ako-nainstalovat-elasticsearch-na-linux-ubuntu-serveri
header_image_file: logo-elastic.png
---

Momentálne pracujem na troch rôznych počítačoch a je ťažké udržiavať ich synchronizované. Samozrejme využívam git repozitáre a cloud ale stále sa musím starať o apache server, databázu, atď. Rozhodol som sa vytvoriť si Virtuálny stroj, ktorý budem mať uložený na USBčku a tým vyriešiť aj tento problém.

===

Inštalácia Linuxu spolu s Apache MySql serverom nie je problém. Ale potreboval som nainštalovať aj Elasticsearch databázu ( ktorá beží na Jave ) a mať ju prístupnú z môjho počítača ( Linux s Elasticsearch bežia vo VM ).

## Inštalácia Javy 8

#### Pridať Java repozitár

Stiahnuť potrebné súbory z Oraclu :

```
$ sudo add-apt-repository -y ppa:webupd8team/java
```

Aktualizácia zoznamu balíčkov :

```
$ sudo apt-get update
```

#### Nainštalovať Javu

```
$ sudo apt-get install oracle-java8-installer
```

Overiť verziu po inštalácii :

```
$ java -version
```

Výstupom by malo byť niečo takéto :

```
java version „1.8.0_72“
Java(TM) SE Runtime Environment (build 1.8.0_72-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.72-b15, mixed mode)
```

## Inštalácia Elasticsearch databázy

#### Pridať elastic repozitár

```
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

#### Vytvoriť zoznam balíčkov pre Elastic

```
$ echo "deb http://packages.elastic.co/elasticsearch/1.7/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-1.7.list
```

Znovu aktualizovať zoznam balíčkov :

```
$ sudo apt-get update
```

#### Nainštalovať Elasticsearch

```
$ sudo apt-get install elasticsearch
```

#### Povoliť Elastic aby sa spúšťal automaticky

```
sudo update-rc.d elasticsearch defaults 95 10
```

Ukážkový výstup :

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

## Konfigurácia

#### Zmena network hostu a povolenie vzdialeného pripojenia

Otvor si Elasticsearch konfiguračný súbor v tvojom obľúbenom editore, povol a zmeň `network.host` na `localhost`. Pokiaľ sa chceš pripájať na Elasticsearch z iného počítača, potrebuješ povoliť `network.bind_host` a nastaviť to na hodnotu `0`.

```
$ sudo vi /etc/elasticsearch/elasticsearch.yml
```

Časť môjho Elasticsearch konfiguračného súbora :

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

#### Reštartovať službu Elasticsearch

```
$ sudo /etc/init.d/elasticsearch restart
```

## Záver

Teraz by si sa už mohol pripojiť a získať dáta z Elasticsearch databázy. Otestovať to môžeš príkazom :

```
$ curl -X GET 'http://localhost:9200'
```

Odpoveď :

```
{
  „status“ : 200,
  „name“ : „Diablo“,
  „cluster_name“ : „elasticsearch“,
  „version“ : {
    „number“ : „1.7.5“,
    „build_hash“ : „00f95f4ffca6de89d68b7ccaf80d148f1f70e4d4“,
    „build_timestamp“ : „2016-02-02T09:55:30Z“,
    „build_snapshot“ : false,
    „lucene_version“ : „4.10.4“
  },
  „tagline“ : „You Know, for Search“
}
```
