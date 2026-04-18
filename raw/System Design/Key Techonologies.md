
1. Core Database
	- most common dbs used are Postgres or dynamoDB
	- do not go into comparing sql with nosql dbs but if asked focus on the differences you are familiar with. eg : I am using postgres because its ACID Properties allow me to maintain high data integrity

2. NoSql Database
	1. contains wide range of data models such as key value, document, column family and graph
	2. often schema less, this allows these dbs to handle large amount of unstructured, semi-structured or structured data and to scale horizontally
	3. strong candidates where:
		- data model is evolving and need to store different types of data structures
		- need to scale horizontally
	4. comes with various consistency models from strong to eventual consistency
	5. also include indexes. most common are b-tree and hash tables
	6. can scale horizontally using consistent hashing and/or sharding
3. Blob storage
	1. Blobs or binary large object represent anything that does not fit neatly into a text or number column
	2. should be stored in a blob storage such as amazon s3 or Google cloud storage
	3. just upload the blob and you get a url which can be used to access that blob
	4. often times these blob storages can work with CDNs. so  you upload your file/blob to the storage which will act like your origin and then CDNs can then cache the file in edge locations
	5. avoid using blob storages as your main db
	6. this is how uploading works
		1. when client uploads a file, they request a presigned url from the server
		2. the server returns a presigned url to which the file will be uploaded. this url is provided by the blob storage itself via the server. the url is stored in the db
		3. client uploads this file to the url ie blob storage
		4. storage triggers a notification when the upload is complete
	7. for downloading
		1. client request a specific file from the server and is returned the presigned url
		2. client uses this presigned url to download  the file via cdn.