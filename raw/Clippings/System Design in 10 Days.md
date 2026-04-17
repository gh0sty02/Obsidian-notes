---
title: "I mastered System Design in 10 days with this step by step plan and cracked every System Design Interview. Here's how you can do it to with this 10 day by day plan."
source: "https://veedaily19.substack.com/p/i-mastered-system-design-in-10-days"
author:
  - "[[Veeraj Kantilal Gadda]]"
published: 2026-04-14
created: 2026-04-17
description: "I have cleared system design interviews at renowned companies like Amazon , iHeartRadio, Apple and I literally did it in 10 days."
tags:
  - "clippings"
---
I have cleared system design interviews at renowned companies like Amazon, iHeartRadio, Apple and I literally did it in 10 days. And here’s the thing. Anyone can do it as long as they have a systematic plan. Also, an important thing is system design doesn’t just mean knowing everything. It means:

1. Knowing exact questions to ask
2. Knowing right concepts
3. And finally framing a perfect answer to give.

Instructions to master System Design that helped me:

1. Make sure the fundamental concepts are learnt and known well.
2. Make sure you have a fundamental list of questions so that you can ask and have a great idea how to answer.
3. Practice with repetition. Here’s how: Read a solution and make sure you understand it. Once you do, think about all the questions and how you would answer it. And then if you can find someone who is also in a tech role and do mock interviews with them. This will not only ensure you are more polished but also where you are lacking.

## Day 1: Master the Framework and Concepts:

### The Universal Question Framework (Memorize This First)

Before every system design question, ask yourself — and your interviewer — these questions in order. Make sure you follow this question framework every time. And the master over 10 days in this will only come with practice.

### Step 1: Clarify Functional Requirements (5 minutes)

*“What does this system actually need to do?”*

- What are the core user actions? (e.g., post, search, upload, pay)
- What’s out of scope for today? (Don’t design everything)
- Who are the users — consumers, businesses, or internal systems?
- Are there any features that seem obvious but may not be required?

### Step 2: Clarify Non-Functional Requirements (5 minutes)

*“What are the quality constraints?”*

- **Scale**: How many DAU? Reads vs. writes per second?
- **Latency**: What’s the acceptable response time? (< 100ms? < 1s?)
- **Consistency**: Does every user need to see the same data instantly, or is eventual consistency acceptable?
- **Availability**: What’s the uptime requirement? (99.9%? 99.99%?)
- **Durability**: Can we lose data? How long must we store it?

### Step 3: Back-of-Envelope Estimation (5 minutes)

*“How big is the problem?”*

- Estimate DAU → requests per second (DAU × avg actions / 86,400 seconds)
- Estimate storage needs (users × avg data size × time)
- Estimate bandwidth (requests/sec × avg payload size)
- Flag the bottleneck: is this read-heavy, write-heavy, or compute-heavy?

### Step 4: Define the API (5 minutes)

*“What does the contract look like?”*

- List 2–4 core API endpoints
- Include inputs, outputs, and key parameters
- This forces you to think in concrete terms before going abstract

### Step 5: High-Level Design (10 minutes)

*“Draw the skeleton”*

- Start with the client, then the API layer, then core services, then data stores
- Don’t over-engineer yet — get the happy path working first
- Walk the interviewer through a single user request end-to-end

### Step 6: Deep Dive on Key Components (15 minutes)

*“Where are the hard parts?”*

- Pick the 2–3 hardest components and go deep
- This is where you show senior-level thinking: tradeoffs, failure modes, scalability bottlenecks

### Step 7: Address Bottlenecks and Failure Modes (5 minutes)

*“What breaks at scale?”*

- Where is the single point of failure?
- What happens when a service goes down?
- How do you handle the 10× traffic spike?

### Day 2: Master these System Design Concepts: Link

- Core Concepts: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#%EF%B8%8F-core-concepts)
- Networking Fundamentals: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#-networking-fundamentals)
- API Fundamentals: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#-api-fundamentals)
- Database Fundamentals: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#%EF%B8%8F-database-fundamentals)
- Caching Fundamentals: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#-caching-fundamentals)
- Asynchronous Communication: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#-asynchronous-communication)
- Distributed System and Microservices: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#-distributed-system-and-microservices)
- Architectural Patterns: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#%EF%B8%8F-architectural-patterns)
- System Design Tradeoffs: [Link](https://github.com/ashishps1/awesome-system-design-resources?tab=readme-ov-file#%EF%B8%8F-system-design-tradeoffs)

## Day 3: Solve three easy questions:

#### 1\. Design a URL shortener (like bit.ly or TinyURL): Detailed Solution

**Functional Requirements:**

- Should we support custom short URLs or only auto-generated ones?
- Do we need analytics (click tracking, geographic data)?
- Should short URLs expire after a certain time?
- Do we need a user account system or anonymous URL creation?
- Should we support editing/deleting URLs after creation?

**Scale Questions:**

- How many URL shortenings per day?
- How many redirections per day?
- What’s the read-to-write ratio? (typically 100:1)
- How long should we store URLs? (Forever? 5 years?)
- Expected URL length for short codes? (6-7 characters?)

**Unique Considerations:**

- How do we handle collision in short URL generation?
- Should we prevent malicious URLs or NSFW content?
- Do we need rate limiting per user/IP?

#### Design a social media feed (like Twitter/X or Facebook): Solution

**Functional Requirements:**

- Core features: post tweets, follow users, view home timeline?
- Should we support: likes, retweets, replies, hashtags, mentions?
- Do we need direct messaging?
- Should we support media (images, videos)?
- Do we need notifications?

**Scale Questions:**

- How many DAU (Daily Active Users)?
- Average tweets per user per day?
- Average follows per user?
- What’s the read-to-write ratio? (typically 1000:1)
- What’s the maximum number of followers a user can have?
- Celebrity accounts with millions of followers - how to handle?

**Unique Considerations:**

- Should the timeline be chronological or algorithm-ranked?
- Real-time updates or eventual consistency acceptable?
- How fresh should the feed be? (seconds? minutes?)
- Pull model (fetch on demand) vs Push model (fanout on write)?

#### 3\. Design a messaging system (like WhatsApp or Facebook Messenger): Solution

**Functional Requirements:**

- One-to-one chat only or group chat too?
- Should we support media sharing (images, videos, files)?
- Do we need read receipts and typing indicators?
- Should we support voice/video calls?
- End-to-end encryption required?
- Message history - how far back?

**Scale Questions:**

- How many DAU?
- Average messages per user per day?
- Average group size?
- Message size limit?
- How long to store messages?

**Unique Considerations:**

- Should messages be delivered in real-time? (< 100ms latency?)
- What happens if user is offline? Store messages for how long?
- Message ordering guarantees?
- Should we support message search?
- Multi-device support?

## Day 4: Solve 1 easy + 2 medium questions

#### Design YouTube/Netflix (Video Streaming): Link

**Functional Requirements:**

- Video upload and playback?
- Should we support: recommendations, comments, likes, subscriptions?
- Live streaming or only pre-recorded videos?
- Do we need content moderation?
- Search functionality?

**Scale Questions:**

- How many DAU?
- How many video uploads per day?
- Average video size and length?
- How many concurrent viewers?
- Total video library size?
- Videos per user per day watched?

**Unique Considerations:**

- Video quality options? (360p, 720p, 1080p, 4K?)
- Adaptive bitrate streaming needed?
- How to handle video transcoding/encoding?
- CDN requirements - global distribution?
- Should we support offline downloads?
- DRM/copyright protection needed?

---

#### 5\. Design Instagram: Solution

**Functional Requirements:**

- Core features: post photos, follow users, view feed, stories?
- Should we support: likes, comments, direct messaging?
- Video support or photos only?
- Story features with 24-hour expiration?
- Do we need filters/editing tools?

**Scale Questions:**

- How many DAU?
- Average posts per user per day?
- Average image size?
- Read-to-write ratio?
- How many followers per user on average?

**Unique Considerations:**

- Image storage and delivery optimization?
- Multiple image sizes (thumbnail, medium, full)?
- Feed ranking algorithm or chronological?
- Real-time story updates?
- How to handle viral posts/celebrities with millions of followers?

---

### 6\. Design Uber/Lyft (Ride-Sharing): Link

**Functional Requirements:**

- Core features: request ride, match driver, track location, payment?
- Should we support: ride scheduling, multiple stops, shared rides?
- Driver-side features: accept/reject rides, navigation?
- Do we need surge pricing?
- Rating system for drivers and riders?

**Scale Questions:**

- How many daily rides?
- How many active drivers and riders?
- Peak vs average traffic ratio?
- Geographic coverage area?
- Average ride duration?

**Unique Considerations:**

- Real-time location tracking frequency? (every 5 seconds?)
- How to match riders with nearby drivers efficiently?
- What happens during high demand? (queueing system?)
- How to handle driver/rider cancellations?
- Payment processing - synchronous or asynchronous?
- How accurate should ETA be?

---

## Day 5: Three medium questions

#### 7\. Design a Web Crawler: Link

**Functional Requirements:**

- What type of content to crawl? (HTML pages, images, PDFs?)
- Should we respect robots.txt?
- Do we need to recrawl pages? How frequently?
- Should we extract and index content?
- Do we need to detect duplicate content?

**Scale Questions:**

- How many web pages to crawl? (billions?)
- Crawl rate? (pages per second?)
- How deep should we crawl? (max URL depth?)
- Storage requirements for crawled data?
- How long to keep crawled data?

**Unique Considerations:**

- How to prioritize URLs? (PageRank, freshness?)
- How to handle dynamic content (JavaScript-rendered)?
- Politeness policy - requests per domain?
- How to detect and handle crawler traps?
- Distributed crawling across regions?

---

#### 8\. Design a Rate Limiter: Solution

**Functional Requirements:**

- What should be rate limited? (API requests, login attempts?)
- Rate limiting rules: per user, per IP, per API key?
- Should we support multiple time windows? (per second, minute, hour?)
- Hard blocking or throttling with queue?
- Do we need different limits for different user tiers?

**Scale Questions:**

- How many requests per second to evaluate?
- How many unique users/IPs?
- Latency requirement for rate limit check? (< 1ms?)
- Distributed system or single server?

**Unique Considerations:**

- What algorithm? (Token bucket, Leaky bucket, Fixed window, Sliding window?)
- How to handle distributed rate limiting across servers?
- Where to place rate limiter? (API gateway, application level?)
- Should we return specific error codes when limit exceeded?
- How to handle clock skew in distributed systems?

---

### Day 6: 2 medium + 1 hard questions

#### 9\. Design a Distributed Cache (Redis/Memcached): Solution

**Functional Requirements:**

- Basic operations: Get, Set, Delete?
- Should we support TTL (time-to-live)?
- Do we need data persistence or in-memory only?
- Should we support complex data types (lists, sets, sorted sets)?
- Do we need pub/sub functionality?

**Scale Questions:**

- How much data to cache? (Total memory size?)
- Expected QPS (queries per second)?
- Average key-value size?
- Cache hit ratio target? (90%? 95%?)
- How many cache nodes?

**Unique Considerations:**

- Eviction policy? (LRU, LFU, FIFO?)
- How to handle cache consistency across nodes?
- Replication strategy for high availability?
- How to handle cache node failures?
- Sharding strategy? (Consistent hashing?)
- Cold start problem - how to warm up cache?

---

### 10\. Design a Key-Value Store (DynamoDB): Link

**Functional Requirements:**

- Operations: Get, Put, Delete, Update?
- Range queries needed or just point lookups?
- Should we support transactions?
- Do we need secondary indexes?
- Batch operations support?

**Scale Questions:**

- How much data to store? (Petabytes?)
- Expected QPS for reads and writes?
- Average key-value size?
- Number of unique keys?
- Data growth rate?

**Unique Considerations:**

- Consistency model: Strong consistency or eventual consistency?
- Availability vs consistency trade-off? (CAP theorem)
- Replication factor?
- How to partition data? (Consistent hashing?)
- How to handle node failures and recovery?
- Do we need to support multiple data centers?

---

#### 11\. Design a Notification System: Link

**Functional Requirements:**

- Notification types: push, SMS, email, in-app?
- Should we support: priority levels, scheduling, user preferences?
- Do we need notification templates?
- Should we support notification grouping/batching?
- Do we need delivery confirmation and read receipts?

**Scale Questions:**

- How many notifications per day?
- Peak notifications per second?
- How many users to notify?
- Notification delivery latency requirement?
- Should we retry failed deliveries?

**Unique Considerations:**

- How to handle users with multiple devices?
- Rate limiting per user to avoid spam?
- How to handle user opt-out preferences?
- Priority queuing for urgent notifications?
- Analytics on open/click rates?
- Third-party service integration (APNs, FCM, Twilio)?

---

### Day 7: 1 medium + 2 hard questions

#### 12\. Design Google Drive/Dropbox: Solution

**Functional Requirements:**

- Core features: upload, download, sync, share files?
- Should we support: folders, file versioning, collaboration?
- Real-time collaboration on documents?
- Do we need file search?
- Should we support offline mode?

**Scale Questions:**

- How many users?
- Average file size?
- How many files per user?
- Total storage capacity needed?
- Upload/download bandwidth requirements?
- Sync frequency for active users?

**Unique Considerations:**

- How to handle large file uploads? (Chunking, resumable uploads?)
- Conflict resolution for simultaneous edits?
- How to detect file changes efficiently? (Delta sync?)
- Compression and deduplication strategies?
- How to handle different file types? (images, videos, documents?)
- Client-side vs server-side sync?
- CDN for faster downloads globally?

---

#### 13\. Design Search Autocomplete/Typeahead: Solution

**Functional Requirements:**

- Should suggestions be personalized?
- Do we need query history?
- Should we support trending/popular queries?
- Language and locale support?
- Should we handle typos and spelling corrections?
- How many suggestions to return? (5? 10?)

**Scale Questions:**

- How many QPS for autocomplete?
- Expected latency? (< 100ms?)
- Size of query vocabulary?
- How many users?
- Update frequency for suggestions?

**Unique Considerations:**

- Data structure for fast prefix matching? (Trie?)
- How to rank suggestions? (popularity, recency, personalization?)
- How to handle new/trending queries?
- Caching strategy?
- Should results update as user types or on debounce?
- How to handle special characters and Unicode?

---

#### 14\. Design a Parking Lot System: Solution

**Functional Requirements:**

- Core features: check in/out, find available spots, payment?
- Different vehicle types? (compact, large, handicapped, electric?)
- Should we support: reservations, multiple entrances/exits?
- Real-time availability display?
- Pricing strategy? (hourly, daily, flat rate?)

**Scale Questions:**

- How many parking spots?
- How many vehicles per day?
- Peak hours capacity?
- Multiple locations or single parking lot?
- Average parking duration?

**Unique Considerations:**

- How to efficiently find nearest available spot?
- Real-time spot availability updates?
- How to handle simultaneous requests for same spot?
- Payment processing - when to charge? (entry, exit, or both?)
- Historical data for analytics and predictions?
- How to handle lost tickets?

---

## Day 8: 3 hard questions

#### 15\. Design a Food Delivery App (DoorDash/Uber Eats): Solution

**Functional Requirements:**

- Core features: browse restaurants, order food, track delivery?
- Should we support: search, filters, ratings/reviews?
- Real-time order tracking?
- Multiple payment methods?
- Do we need a driver assignment system?

**Scale Questions:**

- How many users, restaurants, drivers?
- Orders per day?
- Peak hours traffic?
- Geographic coverage?
- Average delivery time?

**Unique Considerations:**

- How to match orders with available drivers?
- Real-time location tracking for delivery?
- How to handle driver/restaurant availability?
- Estimated delivery time accuracy?
- Surge pricing during peak times?
- How to handle order modifications/cancellations?
- Multi-restaurant orders in one delivery?

---

#### 16\. Design a Hotel Booking System (Booking.com): Solution

**Functional Requirements:**

- Core features: search hotels, view availability, book rooms?
- Should we support: filters (price, amenities), reviews, cancellations?
- Do we need user accounts?
- Should we support multiple room bookings?
- Payment processing - upfront or at hotel?

**Scale Questions:**

- How many hotels in the system?
- How many rooms across all hotels?
- Daily bookings?
- Concurrent users searching?
- Peak season traffic increase?

**Unique Considerations:**

- How to handle inventory management? (avoid double booking)
- Concurrency control for booking same room?
- Should we support overbooking?
- Real-time availability updates?
- How long to hold a room during checkout process?
- Cancellation policies and refunds?
- Different pricing for different dates?
- How to handle time zones?

---

#### 17\. Design a Distributed Task Scheduler: Solution

**Functional Requirements:**

- Task types: one-time, recurring, cron-based?
- Should we support: priorities, dependencies between tasks?
- Do we need task retry logic?
- Should we support task cancellation?
- Do we need task results/logs?

**Scale Questions:**

- How many tasks to schedule per day?
- Average task execution time?
- Number of worker nodes?
- Peak tasks per second?
- Task payload size?

**Unique Considerations:**

- How to ensure tasks run exactly once? (idempotency?)
- How to distribute tasks across workers?
- What happens if a worker dies mid-task?
- Time accuracy requirement? (within seconds? minutes?)
- How to handle failed tasks? (retry policy?)
- Should we support scheduled tasks across time zones?
- Priority queue for urgent tasks?

---

### Day 9: Final 3 hard questions

#### 18\. Design a Leaderboard System: Solution

**Functional Requirements:**

- Leaderboard types: global, regional, friends?
- Real-time updates or periodic refresh?
- Should we support: multiple game modes, time-based periods (daily, weekly)?
- Historical leaderboards?
- Pagination for large leaderboards?

**Scale Questions:**

- How many players?
- Score updates per second?
- Leaderboard size to display? (top 100? 1000?)
- How many different leaderboards?
- Query frequency for leaderboard?

**Unique Considerations:**

- How to efficiently update rankings on score changes?
- Data structure for fast rank retrieval? (Sorted Set in Redis?)
- How to handle tie-breaking?
- Should we show player’s rank even if not in top N?
- How to handle cheaters/invalid scores?
- Cache strategy for frequently accessed leaderboards?
- How to reset leaderboards for new periods?

---

#### 19\. Design a Payment System (PayPal/Stripe): Solution

**Functional Requirements:**

- Core features: process payments, refunds, recurring billing?
- Should we support: multiple currencies, payment methods (cards, wallets, bank)?
- Do we need fraud detection?
- Should we support: payouts, escrow, split payments?
- Transaction history and receipts?

**Scale Questions:**

- Transactions per second?
- Average transaction amount?
- Number of users/merchants?
- Geographic regions to support?
- Peak traffic during sales events?

**Unique Considerations:**

- How to ensure exactly-once payment processing?
- Transaction consistency and atomicity?
- How to handle payment failures and retries?
- PCI compliance requirements?
- How to prevent double charging?
- Reconciliation between different systems?
- Settlement period and timing?
- How to handle chargebacks and disputes?
- Idempotency keys for retries?
- Database transactions vs distributed transactions?

---

#### 20\. Design a Collaborative Document Editor (Google Docs): Solution

**Functional Requirements:**

- Core features: create, edit, save documents?
- Real-time collaboration - multiple users editing simultaneously?
- Should we support: comments, suggestions, version history?
- Offline editing support?
- Rich text formatting?
- Sharing and permissions?

**Scale Questions:**

- How many concurrent users per document?
- Maximum document size?
- Number of active documents?
- Edit operations per second?
- How many versions to store per document?

**Unique Considerations:**

- Conflict resolution for simultaneous edits? (Operational Transform? CRDT?)
- How to show cursor position of other users?
- Latency requirement for seeing others’ changes? (< 100ms?)
- How to handle network partitions?
- Auto-save frequency?
- How to efficiently store and retrieve version history?
- Character-level vs operation-level synchronization?
- How to handle large documents with many collaborators?

### Day 10: Full Revision and Mock Interviews

**Here you simply revise all of the concepts and questions once again and make sure that you do an entire revision. This is enough to prepare you for any interview**

### BEST SYSTEM DESIGN INTERVIEW RESOURCES:

1. **[DesignGuru’s Grokking System Design Course](https://bit.ly/3pMiO8g)**: An interactive learning platform with hands-on exercises and real-world scenarios to strengthen your system design skills.
2. **[Codemia.io](https://codemia.io/?via=javarevisited)**: This is another great platform to practice System design problems for interviews. It got more than 120+ System design problems, many of them are free and also a proper structure to solve them.
3. **[“System Design Interview” by Alex Xu](https://amzn.to/3nU2Mbp)**: This book provides an in-depth exploration of system design concepts, strategies, and interview preparation tips.
4. **[“Designing Data-Intensive Applications”](https://amzn.to/3nXKaas)** by Martin Kleppmann: A comprehensive guide that covers the principles and practices for designing scalable and reliable systems.
5. [LeetCode System Design Tag](https://leetcode.com/explore/learn/card/system-design): LeetCode is a popular platform for technical interview preparation. The System Design tag on LeetCode includes a variety of questions to practice.
6. **[“System Design Primer”](https://bit.ly/3bSaBfC)** on GitHub: A curated list of resources, including articles, books, and videos, to help you prepare for system design interviews.
7. **[Educative’s System Design Cours](https://bit.ly/3Mnh6UR)** e: An interactive learning platform with hands-on exercises and real-world scenarios to strengthen your system design skills.
8. **High Scalability Blog**: A blog that features articles and case studies on the architecture of high-traffic websites and scalable systems.
9. **[YouTube Channels](https://medium.com/javarevisited/top-8-youtube-channels-for-system-design-interview-preparation-970d103ea18d)**: Check out channels like “Gaurav Sen” (ex-Google engineer and founder of [InterviewReddy.io](https://interviewready.io/?aff=JavaRevisited) and “Tech Dummies” for insightful videos on system design concepts and interview preparation.
10. **[ByteByteGo](https://bit.ly/3P3eqMN)**: A live book and course by Alex Xu for System design interview preparation. It contains all the content of System Design Interview book volume 1 and 2 and will be updated with volume 3 which is coming soon.
11. **[Exponent](https://bit.ly/3cNF0vw)**: A specialized site for interview prep especially for FAANG companies like Amazon and Google, They also have a great system design course and many other material which can help you crack FAAN interviews.