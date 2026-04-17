
1. Networking Essentials
	- mostly need to use only HTTP instead of TCP or UDP
	- Websockets when we need bidirectional communication
	- Server Sent Events when we want the server to push data to client such as live scores or notifications
	- only use websockets when required as it add a layer of complexity for maintaining connections`

2. API Design
	- use REST by default
	- map resources to URLs such as /users/{id} for getting user
	- use cursor or offset based pagination
	- in offset based pagination, we query all data and discard which is not needed. offset value is basically the documents which we need to avoid or do not need
	- cursor based pagination requires to use a cursor to indicate where exactly we are in the list of documents and then only query for the next set of documents
	- also might look for rate limit to safeguard from bots and ddos

3. Data Modelling
	- first choice is to use SQL vs NOSQL
	- use SQL if you have structured data with clear relationship and need strong consistency of data. you can use complex queries in SQL and use transactions
	- use NOSql when you need flexible schema or you need to scale horizontally with many servers without join
	- with relational database, there are 2 concepts
		- normalization : splitting data across table to reduce duplication but this means you'll need joins to get data which might become complex and take a hit on performance. 
		- denormalization : allow duplicate data in tables so as to cater faster read operations. downside is updates, if something changes, you'll have to make changes in multiple tables
	- start with normalized data and then transition into denormalization if required

4. Indexing
	- allows faster read operations
	- instead of searching all rows, indexes allows to find the relevant row faster
	- use indexes for columns which are frequently used to query data such as email, userId, etc
	- also might use composite indexes which combine 2 or more columns
	- adding indexes can become expensive as for each addition and removal of row, indexes have to be updated. also indexes are stored on disk so might as well increase disk usage

5. Caching
	- store frequently accessed data in a fast memory like redis so we can skip the db call for reads
	- reduces db load
	- if there is no data related to query in cache then you can query the db as fallback
	- introduces real complexities such as invalidation and cache failures
	- you can invalidate the cache entry immediately after writes, use short Time To Live (TTLs) and accept some staleness or tweak both according to the need
	- what happens when cache fails, all of the traffic is routed to the db. to prevent this you can use a fallback in memory cache and using circuit breaks to prevent overwhelming the db
	- CDN caching is used to cache static assets such as images, videos and js files in edge locations closer to the user
	- inprocess caching is used for small values that change rarely

6. Sharding
	- comes to picture when you outgrow a single db and want to split the data into multiple independent servers
	- important decision is choosing the right sharding key as this will determine how data is distributed in the system
	- eg : if you shard on userId, all of the user data such as profile, posts, likes are stored in a single shard so operations for this is faster while operations which  require aggregation such as finding trending posts might become expensive
	- ![[Pasted image 20260415232834.png]]
	- instead of range based sharding, hash based sharing is generally used which uses following formula `shard = hash(shard_key) % total_shards`
	- a good hash function spreads values uniformly so even if the userids (applied we shard on userid) are consecutive, each userid will be directed to different shard instead of overloading a single shard
	- a big pain point is resharding, which happens if we try to increase the number of shard. if a shard is increased, the hash function changes which results in totally different output. this can be fixed using consistent hashing
	- hash based sharding and consitent hashing both are techinques to find shards to store and retrieve data. hash based uses hashing function using modulo while consistent hashing uses something called ring and keys. when we want to find a shard, we get the key and the move clockwise until we encounter a shard. 
	- adding as shard is even easier.
	- ```
	  Shards: 0°, 180°, 270°, 360°

	  Key at 35° → walk clockwise → first shard hit = 180°  
	  ```

7. CAP Theorem
	- stands for Consistency (all nodes see the same data), Availability (every request gets a response) and Partition Tolerance (System works even when network connections fail between nodes)
	 - comes up when you need to make tradeoffs about how your data behaves during failures
	 - you can only have 2 properties at once
	 - since network partitions are unavoidable in distributed systems, you are really choosing between consistency and availability
	 - Consistency and Partition - correct data but might go down
	 - Availability and Partition - always available but data can be stale
	 - ```
 	   Strong consistency:
		Write comes in → coordinate with all nodes
			   → wait for all to confirm the write
			   → only then respond to user
	   ```
   - ```
	 Eventual consistency:
	  Write to one node → respond immediately
			  → sync to other nodes in background
			  → no guarantee WHEN they sync
			  → but guaranteed they WILL sync eventually
	 ```

![[Pasted image 20260416001035.png]]
