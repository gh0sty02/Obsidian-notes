
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
4. Search Optimized Database
	1. sometimes you will need to implement text search as feature, it is a ability to search through large amount of text data to find relevant results. 
	2. the query is based on exact text matches or results such as 
	3. ```
	   SELECT * FROM documents WHERE document_text LIKE '%search_term%'
	   ```
	4. search optimised databases use techniques such as indexing, tokenization and stemming to make search queries fast and efficient
	5. they use inverted indexes which are data structure that map from words to documents which contain them. 
	6. eg of a inverted index
	7. ```
	   {
		  "word1": [doc1, doc2, doc3],
		  "word2": [doc2, doc3, doc4],
		  "word3": [doc1, doc3, doc4]
		}
	   ```
	8. eg of a search optimized db is ElasticSearch
 
5.  API Gateway
	1. Especially in a microservice architecture, an API Gateway sits at the front of your system to route incoming requests to appropriate Backend service
	2. it is a good practice to have a API gateway in a interview to be a first point of contact for your clients
6. Load Balancer
	1. used to distribute traffic effectively and efficiently so that a single server does not get all the traffic or bottlenecks
	2. the most common decision is to use a load balancer at
		1. level 4 of OSI Model i.e. connection level routing. does not have access to the request data or the data packets. 
		2. level 7 of OSI Model i.e. service layer. it takes a look at the request, the headers and then routes the request to appropriate service which will handle this
	3. Rules of thumb : 
		1. if you have persistent connections such as web sockets, use an L4 load balancer ie connection level load balancer
		2. otherwise use L7 ie service level
	4. Most commonly used load balancer : AWS Elastic Load balancer, NGINX, etc
7. Queue
	1. Serves as a buffer for high or bursty traffic or as a means of distribution of work across systems.
	2. what happens is if a server gets a 1000 request burst and it can handle only 200 of them per second, what queue will do is add them to a queue and once the first 200 are processed, the next 200 will be processed and so on instead of other 800 requests dropping as of inability of the server to handle all requests
	3. this also decouples the producer and the consumer of the system allow them to scale independently.
	4. ![[Pasted image 20260419112743.png]]
	5. things to remember: 
		1. most queues use FIFO however some queues like kafka also allow specific ordering such as ordering based on the priority
		2. many queues have built in retry mechanisms which basically means the queue will try to redeliver the message before considering it as a failure. you can configure this 
		3. dead letter queues are used to store messages that cannot be processed. this helps in debugging and auditing.
		4. queues can be partitioned across multiple servers so as to handle more messages, each partition can be handled by different set of workers
		5. the biggest problem with queues is that it makes it easy to overwhelm the system. if my system supports 200 req per seconds and i receive 300 req per second, i will never be able to finish it. one way to fix this is backpressure which basically means slowing down the production of messages when queue is overwhelmed. 
		   eg : if queue is full, you might want to reject new messages or slow down the rate at which the messages are accepted
	6. most commonly used tech : Kafka
8. Streams/ event sourcing
		1. A stream is nothing but a trail of events until that point of a time
		2. instead of appending a row to the db, you append a event to the log. the current state is derived by replaying all of the events until now
		3. steam is append only so no event from the log can be deleted
		4. eg: 
			1. real time analytics where you'd want to fetch the analytics between a particular time, here we can use the series of events to construct the analytics
			2. event sourcing in critical systems such as banks where you have to keep the trail of how the money got in the account
		5. commonly used tech : Kafka
9. Distribution Lock
	1. when you're dealing with a online system like bookmyshow, when one user is in the middle of buying the ticket, you do not want the other user to grab this ticket so you need some kind of mechanism to ensure this does not happen
	2. traditional dbs with ACID properties use transactions to lock to keep the data consistent but this is only while the the operation ie when the user is updating the record. we want something for long-term locking. this is where distribution lock comes in place
	3. these are perfect to lock something across systems or processes for reasonable amount of time
	4. mostly key value store like redis or zookeeper are used in this. 
	5. eg : if you have a key such as `ticket123`,  if we want to put a lock on this, you can set the value to `ticket123 : locked`, so if another process tries to access it, it will know the key/resource is locked and also it will not be able to set the status to locked as it is already locked. once the operation is finished, the key can be set to unlocked so others can access it.
	6. another handy feature is that they can be set to expire the lock after certain amount of time. this ensures that the lock does not get suck. if a process handling the key is killed, the lock can expire so another process can use it.
	7. ex : 
		1. ecommerce checkout systems use this to put a hold on high demand items while one user is checking it out
		2. ride sharing - a lock can be set to manage the assignment of drivers to riders. a existing driver will have a lock so the rider while requesting does not see this driver
	8. things to remember:
		1. there are multiple ways to implement distributed locking. one common implementation which uses redis is called redlock. redlock uses multiple instances to ensure that the lock is accquired and released in safe and consistent manner
		2. distributed locks can be set to expire after a certain amount of time
		3. can be used to lock a single or a group of resources
10. Distributed Cache
	1. it is just a server or a bunch of servers that stores data in memory for fast retrieval and less latency
	2. useful when:
		1. running expensive tasks queries multiple times - can be cached and the cached data can be returned instead of repeated same computation
		2. reduce number of queries by caching the data which is frequently requested
	3. things to know:
		1. eviction policy - this determines which items should be removed when the cache is full. some of the policies are: 
			1. LRU - removed most recently accessed item first
			2. FIFO - evicts the items in the order they where added
			3. LFU - least frequently used - removed items that were least frequently accessed
		2. cache invalidation statergies - used to ensure that the cache is up to date
		3. cache writing statergy : this makes sure that the data is written in your cache in a consistent way. some of them are : 
			1. write through cache : write to the cache and underlying data store simultaneously
			2. write around cache: writes directly to datastore, this might minimize cache pollution but might increase data fetch times on subsequent reads
			3. write back cache: writes data to the cache first and then asynchronously writes the data to the datastore. fast for write operations but can lead to data loss if the cache fails
	4. don't forget to be explicit about the data you are storing in the cache including the data structure as cache is not just key val pairs.
	5. Redis and memcached are two mostly used cache
11. CDN:
	1. modern systems often serve globally to all users, it is important to deliver content quicky in any part of the world. here is where the CDN comes in.
	2. it is a type of cache which uses distributed systems to deliver data to the user based on geographical location. 
	3. Ofter used for static content such as videos, audio, images and html files
	4. these work by caching the data on servers that are close to the users
	5. when user requests content, the CDN routes the request to the closest server, if the content is cached on the server, it is returned to the user, if it is not cached, CDN will fetch the content from the server, cache it and then return the content to the user
	6. for eg: for a social platform like instagram, it makes sense to cache static assests such as user profile on the CDN
	7. things to remember: 
		1. mostly used for static content and also for dynamic content which is requested frequently but changes infrequently. eg : a blog post which is updated once a day
		2. can be used to cache api responses if the responses are accessed frequently
		3. CDN also have eviction policies which determines which content to remove first. eg : you can se TTL for cached content or use cache invalidation.
	8. most used tech are - cloudflare, akamai and amazon cloudfront