
1. Pushing Real Time Updates
	1. start with a simple HTTP Polling
	2. can also use web sockets and Server Sent Events but that might make infra a bit complicated
	3. for server side Realtime updates, you can use pub-sub which help decouple the publisher and subscriber
2. Managing long running tasks
	1. use queues such as Kafka
	2. when user submits a heavy task, web server instantly validates the request and pushes a job to the queue and return the job id within milliseconds. separate workers processes continuously pull jobs from the queue and execute the actual work
	3. for short running tasks, it makes sense to return the status of the job synchronously with the job id which simples the architecture

![[Common Patterns 2026-04-19 14.27.36.excalidraw]]
3. Dealing with Contention
	1. when users try to access same resource simultaneously, you need a mechanism to prevent race conditions and ensure data consistency
	2. solutions range from db level approaches such as pessimistic locking (before reading, lock first) and optimistic concurrency (don't lock first but verify before commiting) control to distributed coordination mechanisms
	3. queue based solution when instead of letting all users hit the db simultaneously, you funnel the req using queue. a single worker processes them one at time, elimination race conditions entirely
	4. use distributed locks such as redis or zookeeper to act as the lock authority
	5. start with simple DB, scale to distributed only when you genuinely need to
4. Scaling reads:
	1. as app grow, the read traffic becomes larger than the write
	2. this pattern addresses the large volume read requests through db optimization, horizontal scaling and intelligent cachign
	3. optimize for read performance within your db though indexing and denormalization, scale horizontally with read replicas, then add external caching layers such as redis and CDNs
	4. key considerations include managing cache invalidation, handling replication lag in read replicas and dealing with hot keys where millions of users request the same popular content simultaneously
5. Scaling writes
	1. this is addressed through sharding, batching and intelligent load management
	2. core strategies include horizontal sharding (distributed across multiple servers), vertical partitioning (seperating different types of data) and handling write bursts through queues and load shedding
	3. for burst handling, you can use write queues to buffer temporary spikes or implement load sheeding to prioritize important writes during overload.
	4. batching techniques help reduce per operation overhead by grouping multiple writes together. 
6. Handling large blobs
	1. for large blobs, it makes sense to have a client-storage communication with presigned urls and CDNs instead of routing the heavy files through your server
	2. you application generates temporary, scoped credentials (presigned urls) that let clients upload directly to blob storage such as s3. 
	3. downloads come from CDNs with signed URLs for access control, this eliminates your servers as bottlenecks
	4. key challenges includes handling data sync between db metadata and blob storage, handling upload failures and managing lifecycles of large files
7. Multi-Step Processes:
	1. 