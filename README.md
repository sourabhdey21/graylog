# Install graylog server in centos 7.9


yum -y install wget pwgen
yum install -y java-1.8.0-openjdk-headless
java -version

rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

vi /etc/yum.repos.d/elasticsearch.repo

[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

yum install -y elasticsearch

systemctl daemon-reload
systemctl enable elasticsearch

vi /etc/elasticsearch/elasticsearch.yml

Update it like shown below.

cluster.name: graylog

systemctl restart elasticsearch

Give a minute to let the Elasticsearch get fully restarted. Elastisearch should be now listening to 9200 for processing HTTP requests. Use the CURL command to check the response


curl -X GET http://localhost:9200

Output:

{
  "name" : "DF8QK3-",
  "cluster_name" : "graylog",
  "cluster_uuid" : "_wAgUfN9RJeQ0npCKBswVA",
  "version" : {
    "number" : "6.6.0",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "a9861f4",
    "build_date" : "2019-01-24T11:27:09.439740Z",
    "build_snapshot" : false,
    "lucene_version" : "7.6.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}


curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'

output:

{
  "cluster_name" : "graylog",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 0,
  "active_shards" : 0,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}

Install MondoDB

vi /etc/yum.repos.d/mongodb-org-4.0.repo

[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc


yum install -y mongodb-org

systemctl start mongod
systemctl enable mongod


Install Graylog:

rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-3.0-repository_latest.rpm





