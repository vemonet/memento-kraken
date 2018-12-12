# Memento for Data2Services

https://fr.slideshare.net/hvdsomp/dbpedia-archive-using-memento-triple-pattern-fragments-and-hdt

https://github.com/LinkedDataFragments/Server.js/wiki/Deployment-as-Linux-service

LD archive DBpedia: http://fragments.mementodepot.org/

### Convert to HDT

```shell
# Get the java executable and uncompress it
wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/hdt-java/hdt-java-rc2.tgz

# Run script to convert ntriples to HDT
./rdf2hdt.sh /data/drugbank/drugbank-4.5.0.nt /data/drugbank/drugbank-4.5.0.hdt
./rdf2hdt.sh /data/drugbank/drugbank-5.1.1.nt /data/drugbank/drugbank-5.1.1.hdt


# Docker build failing
git clone https://github.com/rdfhdt/hdt-java
docker build -t hdt-java .
docker run -it -v /data/data2services:/data hdt-java -rdftype turtle /data/drugbank-5.1.1.ttl /data/drugbank-5.1.1.hdt
```



### Install Linked Data Server

* https://github.com/LinkedDataFragments/Server.js
* https://github.com/LinkedDataFragments/Server.js/wiki/Configuring-Memento

```shell
docker build -t ldf-server .
docker run -p 3000:3000 -t -i --rm -v /data/data2services:/data -v $(pwd)/config.json:/tmp/config.json ldf-server /tmp/config.json

docker run -p 3000:3000 -t -i --rm -v /data/drugbank:/data -v /home/vemonet/sandbox/memento/config.json:/tmp/config.json ldf-server /tmp/config.json
```

* Query it:

```shell
# Timegate working for us: 
curl -IL -H "Accept-Datetime: Wed, 15 Apr 2013 00:00:00 GMT" http://localhost:3000/timegate/dbpedia?subject=http%3A%2F%2Fdata2services%2Fmodel%2Fgo-category%2Fprocess
# Details in https://github.com/LinkedDataFragments/Server.js/issues/87


# Working on dbpedia
curl -IL -H "Accept-Datetime: Wed, 15 Apr 2013 00:00:00 GMT" http://dbpedia.mementodepot.org/timegate/http://dbpedia.org/page/English
```

