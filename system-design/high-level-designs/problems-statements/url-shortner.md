# Design URL Shortner: Bitly

#### Thinking process
- Functional Requirements 
  - Based on above non functional requirements define the core entities
  - Based on above there is mostly 1:1 api's
- Non Functional Requirements
  - Scaling, latency
  - Do back of envelope calculation later: First design the HLD
- High level Design
- Deep dive into one component

### Functional requirements are of type 
This will be something like 
- User should be able to 
- User can
- User will

### Functional Requirement
- Users can create short url from long url
- Users should be able to redirect to original url
- Users can provide their own custom alias, that is their own short alias
- Url's can have expiration time (like for short period of time)

### Non Functional Requirements
- Latency
  - Redirection should be low latency (Around 200ms)
- Scale
  - 100 million DAU
  - 1 Billion URL's
- Ensure uniqueness of short code
- CAP
  - Partition tolerance is always there
  - Availability in this case: Because we dont need strong consistency as user will take time to share the url 
  and clicks will come.


#### Core Entities
- Original URL
- Short URL
- User

#### API
- createShortURL: /url, POST -> shortURL
{
    originalUrl,
    alias (Optional),
    expirationTime (optional)
}

- redirectURL: /{shortURL}, GET -> redirect to original url
  - Returns 302 redirect(temporary redirect) as we are redirecting
  - Note: 301 is permanent redirect (in this browser caches it as it is permanent redirect, hence call may not come
  to your servers)


#### Database Models

URL Table:
- Short url / custom alias (Primary key)
- Long url
- createdTime
- expirationTime
- UserId

User:
- UserId
- .....

#### Design Diagram
[ticket-master.excalidraw](design-diagrams/ticket-master.excalidraw)

#### Key points:
1. Create Short URL from long URL

Note: We need something between length of 5 to 7

Approaches
- Random number generator
  - Generate Random number between 1 and 1Billion
  - We can do base 62 encoding of above number: DOES NOT WORK (Has collision, to avoid collision, 
  check and then write to DB)
- Instead of random ID generator go with _unique ID Generator_
  - Do base 62 encoding of it
- Hash the long url
  - md5, sha 256(long url) -> hash -> base62(hash)[:6]
- Counter
  - increment a counter -> then do base 62 encoding (Always do this for it to be short)
  - CON: This has predictability
  - CON: if we have multiple write service instance then we need global counter, it could be in REDIS
  In redis it is called INCR
- 

2. How would we define database
   - short url (primary key)
   - long url (create secondary index on this)

3. To make it fast if we dont want to hit the cache
   - Add read through cache
   - LRU cache
   - Key: shortURL, value: LongURL

4. To further cache:
   - Add it to CDN

5. No of reads or redirects are greater than creation of url. Hence no of reads are greater than writes. 
Hence, we can create read service and write service separately, hence we can make microservice architecture.
However, having in the same box can also be possible.

6. Database:
   - Short code: 8 bytes
   - Long code: 100 bytes
   - creation time: 8 bytes always
   - custom alias: 100 bytes
   - expiration time: 8 bytes
   - TOTAL: 224 Bytes =~ 500Bytes with buffer 
   
Total storage size:
500 Bytes * 1 Billion = 500 GB

Most of the database can handle 1 TB of data on single instance. 
However if we still want to do it for writes, we can shard it based on short url, just using hashing algorithm
We can also have simple database scaling based on read replicas


#### Resources
- [Hello interview youtube](https://www.youtube.com/watch?v=iUU4O1sWtJA)
- [Hello interview content](https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly)
- Alex XU book



