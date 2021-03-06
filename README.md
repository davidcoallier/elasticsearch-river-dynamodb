DynamoDB River Plugin for Elasticsearch
==================================

[![Build Status](https://travis-ci.org/kzwang/elasticsearch-river-dynamodb.png?branch=master)](https://travis-ci.org/kzwang/elasticsearch-river-dynamodb)

The DynamoDB River plugin allows to fetch data from [DynamoDB](http://aws.amazon.com/dynamodb/) and indexing into Elasticsearch

In order to install the plugin, simply run: `bin/plugin -install com.github.kzwang/elasticsearch-river-dynamodb/1.0.0`.


|       DynamoDB Plugin       |    elasticsearch    | Release date |
|-----------------------------|---------------------|:------------:|
| 1.1.0-SNAPSHOT (master)     | 1.0.0.RC2           |              |
| 1.0.0                       | 1.0.0               | 2014-02-13   |
| 1.0.0.RC1                   | 1.0.0.RC2           | 2014-02-10   |


## Create River
```sh
curl -XPUT 'localhost:9200/_river/dynamodb_test/_meta' -d '{
    "type" : "dynamodb",
    "dynamodb" : {
        "access_key" : "YOUR AWS ACCESS KEY",
        "secret_key" : "YOUT AWS SECRET KEY",
        "table_name" : "testdb",
        "id_field" : "my_id",
        "interval" : "5s",
        "updated_timestamp_field" : "updated",
        "deleted_timestamp_field" : "deleted"
    }
}'
```


## Settings
|  Setting                            |   Description
|-------------------------------------|----------------------------------
| access_key                          | AWS access key **Mandatory**
| secret_key                          | AWS secret key **Mandatory**
| table_name                          | Table name in DynamoDB for index **Mandatory**
| region                              | AWS regoin for the DynamoDB. Defaults to `us-west-2`
| index                               | Index name in Elasticsearch. Defaults to same as *table_name*
| type                                | Type name in Elasticsearch. Defaults to same as *table_name*
| interval                            | Index interval, set to `0s` to index once only. Default to `0s`
| bulk_size                           | Bulk size for fetching from DynamoDB and index in to Elasticsearch. Defaults to `100`
| id_field                            | Field name in DynamoDB for the id field. Defaults to `id`
| updated_timestamp_field             | Field name in DynamoDB of item updated timestamp
| deleted_timestamp_field             | Field name in DynamoDB of item deleted timestamp


#### Updated Timestamp Field
Name of the field which contains the timestamp of when the item updated. This is mandatory if you want only index new items since last index.

#### Deleted Timestamp Field
Name of the field which contains the timestamp of when the item deleted. This is mandatory if you want DynamoDB river automatically delete items in the index.