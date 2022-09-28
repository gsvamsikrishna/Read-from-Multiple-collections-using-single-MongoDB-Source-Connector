# Read-from-Multiple-collections-using-single-MongoDB-Source-Connector

![image](https://user-images.githubusercontent.com/73946498/192213664-38a50cd1-8da8-4f6f-bd3d-b6b0803d987d.png)


[{"$match": {"ns.coll": {"$regex": /^(sink-collection|test-collection)$/}}}]

