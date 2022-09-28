# Read from Multiple MongoDB Collections using a Single MongoDB Source Connector

When configuring MongoDB Source connector on Confluent Cloud UI or using JSON file in case of Self-managed Connectors, you can see a property called "Collection".

![image](https://user-images.githubusercontent.com/73946498/192697206-fdb90c45-69a8-4e3b-84bb-7b24cc76cef8.png)
![image](https://user-images.githubusercontent.com/73946498/192697290-eb803f7f-cdf5-439c-ad86-ec3e0bf73c62.png)

Based on documentation the definition and usage of this property is 

Collection: Name of the collection in the database to watch for changes. If not set, the connector watches all collections for changes.

You can only set one collection name as value for this property.  When left blank, the connector will listen to all collections inside the Database.
#### What is the solution for listening from morethan one collection, but not all collections?

## Enter "Pipeline"

My MongoDB Cluster has 2 Databases and in "test-db" Database, there are 5 Collections
![image](https://user-images.githubusercontent.com/73946498/192702254-217b8209-4399-4f06-b218-b1335657d62f.png)

While Configuring the connector, leave the "Collection" property blank
![image](https://user-images.githubusercontent.com/73946498/192702805-68889e5d-40c2-4d3c-b8b7-57551c9cb30d.png)

Pipeline property needs to have value in MongoDB Pipeline format: [{"$match": {"ns.coll": {"$regex": /^(aTest|testCollection)$/}}}]
![image](https://user-images.githubusercontent.com/73946498/192703152-7123751e-da4c-499a-9144-af121c24d133.png)


Definition and usage of Pipeline Property is explained here: https://docs.confluent.io/cloud/current/connectors/cc-mongo-db-source.html#connection-details and https://www.mongodb.com/docs/kafka-connector/current/source-connector/configuration-properties/all-properties/#change-streams

Pipeline: An array of JSON objects describing the pipeline operations to filter or modify the change events output. For example, [{“$match”: {“ns.coll”: {“$regex”: /^(collection1|collection2)$/}}}] will set your source connector to listen to the “collection1” and “collection2” collections only.

More info on MongoDB Aggregations can be found at https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/

On the Confluent Cloud Console -- Topics view, there are only two topics created with "test-db" prefix and both are populated with data from respective MongoDB Collections.

![image](https://user-images.githubusercontent.com/73946498/192704389-e7b6c982-2d8f-48ec-84d5-5ba25f923103.png)

### References
- https://docs.confluent.io/cloud/current/connectors/cc-mongo-db-source.html#connection-details 
- https://www.mongodb.com/docs/kafka-connector/current/source-connector/usage-examples/multiple-sources/
- https://www.mongodb.com/docs/kafka-connector/current/source-connector/usage-examples/custom-pipeline/


You can also create a Pipeline to read from morethan One Database + morethan One Collection combination.
Hint: [{"$match": {"ns.db": {"$regex": /^(abc|test-db)$/}, "ns.coll": {"$regex": /^(abcTesting|txns)$/}}}]
