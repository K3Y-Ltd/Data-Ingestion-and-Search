![ACROSS](/docs/logo_across.jpg)

# Data Ingestion and Search (DIS)

![Architecture](/docs/DIS.jpg)

This repository holds the required documentation for running the Data Ingestion (DIS) and Search which heavily relies on Elasticsearch for storing data in a search Engine. Additionally, a visualization dashboard utilizing Kibana is also provided. This service has been originaly developed and deployed during the ACROSS project. It is also deployed with TLS certificates to ensure E2E encryption.

## Prerequisite
In order to run and the deploy DIS the following must be met:
- docker (engine)
- docker-compose
- openssl (might already be installed in linux)

## Installation
The installation process shall follow the steps provided bellow. However before proceeding you must create a `.env` file that has similar content as `.env.local`. Once you have done that follow these steps:

1) Change the following environment variables:
   1) Change the `ELASTIC_USERNAME` and `ELASTIC_PASSWORD` environment variables for the ADMIN user that will be used by elasticsearcha and kibana
   2) Change the `XPACK_ENCRYPTION_KEY` with a long string of random values that will be used to encrypt information
   3) Change the `CA_PASSWORD` that will be used to create the certificates
2) Create the certificates:
   1) Execute the `docker-compose -f docker-compose.setup.yml run --rm certs` which will spin a container to create the certificates and then remove it.
3) **Optional**: Change the allocated heap size for elasticsearch by changing the value in the `docker-compose.yml` file:
```docker-compose
ES_JAVA_OPTS: -Xmx5g -Xms5g
```
Currently 5GB are allocated which is sufficient to run elasticsearch
4) Execute to build DIS along with Kibana `docker-compose up -d`

## Use
After the 2 containers that have been created:
- Elasticsearch
- Kibana

The exposed ports that someone can interface for Elaticsearch are `9200` and `9300`, for the dashboard `5601`.

Open a browser and specify the `https://{IP_ADDRESS}:5601`, provide the environment variables you provided in order to loging.

You can create users using the Kibana dashboard in case you want more users other than the admin you created.

You can also test the connection with the running instance of DIS remotely with the following command 
```bash
curl -k https://{IP_ADDRESS}:9200 -u {ELASTIC_USERNAME}:{ELASTIC_PASSWORD}
```

and expect the following response:
```json
{
  "name" : "elasticsearch",
  "cluster_name" : "elk-tls-cluster",
  "cluster_uuid" : "dengBDquRHmgScK8GH_0Fw",
  "version" : {
    "number" : "7.16.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "6fc81662312141fe7691d7c1c91b8658ac17aa0d",
    "build_date" : "2021-12-02T15:46:35.697268109Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

![K3Y Ltd](/docs/logo_k3y.png)