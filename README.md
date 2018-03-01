# Dynamo-to-ElasticSearch

A NPM module that will dump all DynamoDB data to AWS ElasticSearch indices.

## What this does

It takes entire table from DynamoDB and injects it to AWS Elasticsearch service. It may serve as a blueprint in AWS Lambda, however it is not based on streams, but on DynamoDB scan operation.

This module is an extended version of [dynamodb-to-elasticsearch](https://www.npmjs.com/package/dynamodb-to-elasticsearch). The difference is that here you can set your own `primaryKey` as well as index type. It is a separate repository as it breaks backward compatibility.

---

### Installation

```sh
$ npm install dynamo-to-elasticsearch
```

## [Guide to configure AWS to use this blueprint](https://aws.amazon.com/blogs/compute/indexing-amazon-dynamodb-content-with-amazon-elasticsearch-service-using-aws-lambda)

### Documentation

`module.exec (table, region, es_endpoint, es_data = { id: 'sortKey', type: 'datatype', indiceName: ''})`

| Parameter | Type | Description
| ------ | ------ | ------ |
| table | string | Table name of dynamoDB whose data you want to dump in elastic-search.
| region | string | dynamodb table region
| es_endpoint | string | elastic-search endpoint
| es_data | object |
| es_data.id | string | The name of `primaryKey` field for DynamoDB table. It should be unique identifier for documents. By default it is set to `sortKey`. 
| es_data.type | string | Name of class of objects, which document represents. By default it is set to `datatype`.
| es_data.indiceName | string | Index name of elastic-search on which you can perform query.

### Example

```javascript
const d2es = require('dynamodb-to-elasticsearch');

const table = 'table',
	region = 'region',
	es_endpoint = 'es_endpoint_value',
	es_data = { id: 'sortKey', type: 'data', indiceName: 'candidates' }};

exports.handler = function(event, context, callback) {
	d2es.exec(table, region, es_endpoint, es_data, (err, success) => {
	    if (err) {
			callback(err, null);
		} else {
			callback(null, success);
		}
	});
}

```

License
----
MIT
