## ElasticSearch

https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html

docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.11.3
docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.11.3
<!--- generate password; store it in an environment variable with export ELASTIC_PASSWORD="your_password" -->
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
<!--- use this to generate an enrollment token for kibana -->
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
curl --cacert http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200


docker pull docker.elastic.co/kibana/kibana:8.11.3
docker run --name kibana --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.11.3


every instance of elasticsearch runs as a node
connected nodes = cluster
all nodes can handle HTTP request (REST clients) and transport (communication between nodes)

create documents with POST <<index>>/_doc (single) or POST /_bulk
every document includes an unique _id when generated

get documents with GET <<index>>/_search

POST books/_doc
{"name": "Snow Crash", "author": "Neal Stephenson", "release_date": "1992-06-01", "page_count": 470}

POST /_bulk
{ "index" : { "_index" : "books" } }
{"name": "Revelation Space", "author": "Alastair Reynolds", "release_date": "2000-03-15", "page_count": 585}
{ "index" : { "_index" : "books" } }
{"name": "1984", "author": "George Orwell", "release_date": "1985-06-01", "page_count": 328}
{ "index" : { "_index" : "books" } }
{"name": "Fahrenheit 451", "author": "Ray Bradbury", "release_date": "1953-10-15", "page_count": 227}
{ "index" : { "_index" : "books" } }
{"name": "Brave New World", "author": "Aldous Huxley", "release_date": "1932-06-01", "page_count": 268}
{ "index" : { "_index" : "books" } }
{"name": "The Handmaids Tale", "author": "Margaret Atwood", "release_date": "1985-06-01", "page_count": 311}

GET books/_search

GET books/_search
{
  "query": {
    "match": {
      "name": "brave"
    }
  }
}
