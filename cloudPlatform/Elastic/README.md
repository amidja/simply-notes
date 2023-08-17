
# Elastic 


##  References 
[Home](https://www.elastic.co/)
[Cloud Elastic](https://cloud.elastic.co/deployments)
[GitHub Home](https://github.com/elastic/elasticsearch/tree/8.8)
[Installing Elastic Stack Locally](https://www.elastic.co/guide/en/elastic-stack/8.8/installing-elastic-stack.html)
[Installing Elastic Search in Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)

##  Tutorials 

[Logoz ElasticSearch Tutorial](https://logz.io/blog/elasticsearch-tutorial/)

## Elastic in Docker

[Installing Elastic in Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)

ℹ️ Starting a single node in Docker
docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.9.0
docker run --name es01 --net elastic -p 9200:9200 -it docker.elastic.co/elasticsearch/elasticsearch:8.9.0


ℹ️ Starting a single node in Docker
This will generated pwd for the 'elastic' user and output to the terminal, plus an enrollment token for enrolling Kibana.

ℹ️ To reset pwd use 'elasticsearch-reset-password' tool
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password

ℹ️ To verify connect to the Elasticsearch cluster

docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
curl --cacert http_ca.crt -k -u elastic https://localhost:9200

## Kibana in Docker
[Kibana in Docker](https://www.elastic.co/guide/en/kibana/8.9/docker.html)

ℹ️ Starting Kibana Docker
docker pull docker.elastic.co/kibana/kibana:8.9.0
docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.9.0

## Elastic Search Clients

[Home](https://www.elastic.co/guide/en/elasticsearch/client/index.html)

## Training 

### Cloude Elastic Training 

Lab Credentials 
    > username: training
    > password: nonprodpwd 
    
## Metrics

[K8 Pod Metrics](https://www.elastic.co/guide/en/observability/current/kubernetes-pod-metrics.html)

## Monitoring Java App With Elastic 

https://www.elastic.co/blog/monitoring-java-applications-and-getting-started-with-the-elastic-apm-java-agent

java -javaagent:/home/elastic/petclinic/elastic-apm-agent-1.21.0.jar \
  -Delastic.apm.service_name=petclinic-spring \
  -Delastic.apm.server_urls=https://4d6c92f932664ca29d6ddca25c05de4f.apm.us-central1.gcp.cloud.es.io:443 \
  -Delastic.apm.secret_token=hL16Y8HySf2el8l9j4 \
  -Delastic.apm.environment=production \
  -Delastic.apm.application_packages=org.springframework.samples.petclinic \
  -jar /home/elastic/petclinic/spring-petclinic-1.5.16.jar