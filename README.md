# elastic-search

## Configurando. 

After provisioning the databse I created a set of credentials to use them on the kibana config file.
I followed this set of [instructions](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started) from IBM Cloud. 
- Remember to create an admin password under IBM Cloud Setting Menu.
- First I had to create the kibana file
```yml
elasticsearch.ssl.certificateAuthorities: "/usr/share/kibana/config/cacert"
elasticsearch.username: "<USERNAME>"
elasticsearch.password: "<PASSWORD>"
elasticsearch.hosts: ["https:<USERNAME>:<PASSWORD>@<HOSTNAME>:<PORT>"]
server.name: "kibana"
server.host: "0.0.0.0"
```
Where:
`USERNAME`: I got it from IBM Cloud credentials.
`PASSWORD`: Got it from IBM Cloud Credentials.
`HOSTS`: The compoosed hosts fiels is based on the username, password, hostname and port taken from the IBM Cloud credentials.

Also from IBM Cloud downloaded the Cert file for the conection with the database and store it under created directory `certs` with name `db.crt`.

After having the config files I ran the docker container:
Generic command:
```bash 
docker container run -it --name kibana \
-v </path/to/kibana.yml>:/usr/share/kibana/config/kibana.yml \
-v </path/to/cacert>:/usr/share/kibana/config/cacert \
-p 5601:5601 docker.elastic.co/kibana/kibana-oss:<kibana_version>
```
how it looked:
```bash 
docker container run -it --name kibana \
-v ~/git/elastic-search/kibana.yml:/usr/share/kibana/config/kibana.yml \
-v ~/git/elastic-search/certs/db.crt:/usr/share/kibana/config/cacert \
-p 5601:5601 docker.elastic.co/kibana/kibana-oss:7.10.0
```
**NOTE:** make sure you enter directorie and not only filenames.
**Friendly Reminder:** You can view Kibana's version compatible with Elasticsearch on this [matrix](https://www.elastic.co/es/support/matrix#matrix_compatibility) and on this other
[site](https://www.docker.elastic.co/r/kibana) you will the docker images repository from elastic.
If you set it up properly you might get an output like the following:
```bash
  log   [02:04:24.188] [info][savedobjects-service] Starting saved objects migrations
  log   [02:04:25.088] [info][savedobjects-service] Creating index .kibana_1.
  log   [02:04:25.652] [info][savedobjects-service] Pointing alias .kibana to .kibana_1.
  log   [02:04:26.379] [info][savedobjects-service] Finished in 1598ms.
  log   [02:04:26.393] [info][plugins-system] Starting [40] plugins: [usageCollection,telemetryCollectionManager,telemetry,kibanaUsageCollection,mapsLegacy,securityOss,newsfeed,kibanaLegacy,share,legacyExport,embeddable,expressions,data,home,console,apmOss,management,indexPatternManagement,advancedSettings,savedObjects,dashboard,visualizations,visTypeVega,visTypeTimelion,timelion,visTypeTable,visTypeMarkdown,tileMap,regionMap,inputControlVis,visualize,esUiShared,charts,visTypeVislib,visTypeTimeseries,visTypeTagcloud,visTypeMetric,discover,savedObjectsManagement,bfetch]
  log   [02:04:26.627] [info][listening] Server running at http://0.0.0.0:5601
  log   [02:04:26.661] [info][server][Kibana][http] http server running at http://0.0.0.0:5601
```
When you get that kind of output you will be able to access the kibana interface at localhost.
