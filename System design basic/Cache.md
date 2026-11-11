# Types of Cache

> **TL;DR:** A **cache** is a small, fast storage area that keeps copies of data you use often, so you don't have to fetch it from a slower place every time. There are four main types: **CPU cache** (inside the processor), **disk cache** (in RAM, speeds up disk access), **web cache** (stores web content), and **DNS cache** (stores website address lookups). All four work the same way: check the cache first, and only go to the slow source if the data isn't there.

---

## The Simple Idea

Imagine you cook every day. Instead of walking to the market each time you need salt, you keep a small jar of salt on your kitchen counter.

- **The market:** slow, far away, has everything (main memory, disk, remote server).
- **The jar on the counter:** fast, close, holds only what you use most (the cache).

Caching means keeping copies of frequently used data close by, so getting it is fast.

### Two words you must know

| Term | Meaning | Analogy |
|---|---|---|
| **Cache hit** | The data you need is already in the cache. Fast. | The salt is in the jar. |
| **Cache miss** | The data is not in the cache, so you must fetch it from the slow source, then usually save a copy in the cache. | The jar is empty, so you go to the market and refill it. |

The basic flow for every type of cache:

```
Need data
   |
   v
Check cache
   |
   +-- Found (hit)      --> Return it quickly
   |
   +-- Not found (miss) --> Fetch from slow source
                            --> Save a copy in cache
                            --> Return it
```

---

## What Is Cache?

A cache is a high-speed data storage mechanism that temporarily stores frequently accessed or recently used data and instructions.

In a computer, it sits between the main memory (RAM) and the central processing unit (CPU). By keeping frequently used data closer to the CPU, the cache gives quicker access and reduces the delay (latency) of fetching data from slower main memory.

The same idea is used far beyond the CPU, in disks, web browsers, networks, and DNS. That is why there are different types of cache.

---

## Importance of Caching

Caching improves performance and efficiency in many computing environments. Here is each benefit explained:

### 1. Faster data access
Frequently used data is stored closer to the processor, so the system spends less time fetching it from slower storage such as disk drives or network servers. Data retrieval is faster and the system feels more responsive.

### 2. Reduced latency
Latency is the delay between asking for data and receiving it. Caching reduces this delay because commonly used data is kept ready, instead of being fetched from distant or slow devices.

### 3. Bandwidth conservation
When popular content is served locally, it does not need to be downloaded again and again from remote servers. This saves network bandwidth, reduces load on the network, and improves overall network performance.

### 4. Scalability
Caching takes work away from backend systems (like databases and servers). Since many requests are answered from the cache, the system can handle more users without slowing down.

### 5. Improved user experience
Faster access and less waiting means smoother browsing, quicker file access, and apps that feel responsive. Users are happier.

### 6. Enhanced reliability
During heavy demand or network problems, cached content can still be served. This helps keep the service available and reduces the effect of failures.

### Summary table

| Benefit | What it means |
|---|---|
| Faster data access | Data comes from a nearby, fast place |
| Reduced latency | Less waiting time |
| Bandwidth conservation | Fewer repeated downloads |
| Scalability | Backend handles more users |
| Better user experience | Smooth and responsive |
| Enhanced reliability | Service keeps working under stress |

---

## Types of Cache

The four types covered:

| # | Type | Where it lives | What it speeds up |
|---|---|---|---|
| 1 | **CPU cache** | On the processor chip | Access to data and instructions |
| 2 | **Disk cache** | In RAM | Reading and writing to disk |
| 3 | **Web cache** | Browser, proxy, or CDN servers | Loading web content |
| 4 | **DNS cache** | Client devices or DNS servers | Finding a website's IP address |

---

## 1. CPU Cache

CPU cache is a core part of modern processors. It reduces the time needed to access frequently used data and instructions.

### Characteristics

**Proximity to the CPU**
The CPU cache is located directly on the processor chip. That makes access extremely fast compared with fetching data from main memory (RAM).

**Hierarchical structure**
CPU cache comes in multiple levels (L1, L2, L3). Each level differs in size, speed, and distance from the CPU cores.

```
CPU Core
   |
   v
 L1 Cache   (smallest, fastest, closest)
   |
   v
 L2 Cache   (bigger, slower)
   |
   v
 L3 Cache   (biggest, slowest of the three)
   |
   v
 Main Memory (RAM)   (much bigger, much slower)
```

### Functionality

**Data and instruction caching**
- The CPU cache stores both frequently used data and instructions.
- When the CPU needs something, it checks the cache first.
- If found (cache hit), it is retrieved quickly.
- If not found (cache miss), the CPU gets it from main memory and stores a copy in the cache for next time.

**Cache coherency**
Modern CPUs have several cores, and each core may have its own cache. If two cores keep a copy of the same data and one changes it, the other copy becomes outdated. Cache coherency protocols solve this. They keep data consistent between cache levels and CPU cores, preventing errors when multiple cores read and modify the same data.

### Types of CPU cache

| Level | Details |
|---|---|
| **L1 cache** | Split into an instruction cache (L1i) and a data cache (L1d). Extremely fast but small, typically a few KB up to a few MB per core. |
| **L2 cache** | A secondary cache. Larger than L1 but slower. The article says it is shared among CPU cores within a single core complex or chip. |
| **L3 cache** | The last level of the hierarchy. Shared among multiple cores, or across multiple CPU sockets. Larger than L2 but slower. |

### Use cases and benefits

- **Improved performance:** programs run faster and the system feels more responsive.
- **Reduced memory latency:** data is near the CPU cores, so there is less delay fetching from main memory.
- **Higher throughput:** the CPU stalls less often waiting for data, so it executes instructions more efficiently.
- **Optimized multithreading:** in programs using many threads, cache helps reduce contention for shared resources and avoids cache thrashing (cache entries being constantly replaced and reloaded).

---

## 2. Disk Cache

Disk cache, also called **buffer cache**, improves disk input/output (I/O) performance by temporarily storing recently accessed data in memory.

Disks are slow compared with RAM, so keeping recently used disk data in RAM saves a lot of time.

### Characteristics

**RAM-based storage**
Disk cache uses part of the computer's RAM to temporarily hold data blocks that were recently read from or written to the disk.

**Write-back or write-through**

| Mode | How it works | Trade-off |
|---|---|---|
| **Write-back** | Data is written to the cache first, and flushed to disk later. | Faster, but data could be lost if power fails before flushing. |
| **Write-through** | Data is written to the cache and the disk at the same time. | Safer, but slower. |

**Transparent operation**
Disk cache works behind the scenes. The operating system and applications don't need to do anything. It intercepts disk requests and caches data blocks automatically.

### Functionality

**Read caching**
- Recently read data blocks are kept in memory.
- When a block is requested, the cache checks if it is there (cache hit) and returns it quickly.
- If not (cache miss), the block is read from disk and stored in the cache for next time.

**Write caching**
- In write-back mode, data is first written to the cache and the application is told the write is done. This reduces write latency.
- The data is flushed to disk later, asynchronously (in the background).
- This improves application responsiveness.

**Prefetching**
- The cache can load data blocks into memory before they are requested, based on access patterns or prediction.
- By guessing what will be needed next, it reduces disk access delays. For example, if you read the first part of a file, the cache may load the next part in advance.

### Types of disk cache

| Type | Details |
|---|---|
| **Operating system cache** | Many operating systems keep a disk cache as part of the file system. The kernel manages it to buffer disk I/O. |
| **Disk controller cache** | Some disk controllers have built-in cache memory. It works independently of the OS cache and may have battery backup to protect cached data if power is lost. |
| **Disk cache software** | Third-party software can add to or replace the OS cache. It often offers advanced caching algorithms and more settings to tune performance. |

### Use cases and benefits

- **Improved disk performance:** less I/O delay, so reads and writes are faster.
- **Enhanced system responsiveness:** applications and the system feel faster.
- **Reduced disk wear:** fewer disk accesses means less wear and tear, which may extend the disk's life.
- **Optimized resource utilization:** fewer disk accesses frees up CPU and disk bandwidth for other tasks.

---

## 3. Web Cache

Web cache, also called **web proxy cache**, stores copies of web content (HTML pages, images, multimedia files) locally on servers or client devices.

### Characteristics

**Proxy-based storage**
A web cache acts as a go-between for clients (such as web browsers) and web servers. It intercepts requests and caches the content that clients ask for.

**Caching policies**
Rules decide what to cache and for how long. They may depend on how popular the content is, how fresh it is, and how much cache space is available.

**Hierarchical caching**
In large deployments, caches can be layered. Lower-level caches serve individual clients. Higher-level caches sit between the lower-level caches and the web servers.

```
Browser  -->  Local cache  -->  Higher-level cache  -->  Web server
```

### Functionality

**Content caching**
- The cache stores copies of web content that clients requested.
- When a client asks for a resource, the cache checks for a copy (cache hit).
- If found, it serves the copy directly and the original web server is not contacted.
- If not found (cache miss), the cache fetches it from the server and stores a copy for future requests.

**Cache invalidation**
Cached content can go stale (outdated). The cache needs to make sure clients get the latest version. It does this by checking HTTP headers such as `Last-Modified` or `ETag` and revalidating the content with the origin server.

**Content compression**
Some web caches compress content using techniques such as gzip or brotli. This saves storage space and makes the cache more efficient.

### Types of web cache

**Proxy cache**
- Placed at network edges or inside an organization's network.
- Intercepts and caches web content requested by clients.
- Reduces latency and bandwidth usage.

**Browser cache**
- Your web browser keeps its own local cache.
- It stores previously visited pages, images, and other files.
- When you visit the same site again, content loads from local storage, which is faster.

**CDN (Content Delivery Network) cache**
- A CDN is a network of edge servers spread across many locations around the world.
- These edge servers cache copies of web content.
- Users get content from the server nearest to them, which reduces delay and speeds up delivery.

| Web cache type | Where it lives | Who benefits |
|---|---|---|
| **Browser cache** | Your own device | You, on repeat visits |
| **Proxy cache** | Organization or network edge | Everyone on that network |
| **CDN cache** | Edge servers around the world | Users everywhere, based on location |

### Use cases and benefits

- **Faster web browsing:** pages load quicker because content comes from the cache.
- **Bandwidth conservation:** less repeated downloading from remote servers, so less internet traffic.
- **Improved website performance:** servers have less load because cache servers handle delivery, improving scalability and responsiveness.
- **Reduced latency:** nearby content means faster page loads and a better user experience.

---

## 4. DNS Cache

DNS cache, also called **resolver cache**, temporarily stores recently looked-up domain name to IP address mappings.

**First, what is DNS?** Computers find websites using IP addresses (like 142.250.80.46), but people use names (like google.com). DNS (Domain Name System) is the system that translates names into IP addresses. Think of it as the internet's phone book.

A DNS cache keeps recent lookups so the phone book doesn't have to be searched every time.

### Characteristics

**Local storage**
The DNS cache lives on client devices or DNS servers. It keeps recently resolved mappings for a configurable period.

**Time-to-Live (TTL)**
Each cached DNS record has a TTL value that says how long it can be kept. When the TTL runs out, the record is discarded. The resolver must then ask the authoritative DNS servers (the official source) for fresh information.

**Hierarchical caching**
Local DNS caches serve individual devices. Intermediate DNS servers cache results on behalf of many clients.

```
Your device cache --> ISP / public DNS server cache --> Authoritative DNS server
```

### Functionality

**DNS resolution caching**
- Recent name-to-IP mappings are stored locally.
- When you request an IP for a domain, the resolver first checks its cache (cache hit).
- If found, it returns the IP immediately without asking external servers.
- If not found (cache miss), it asks the authoritative DNS servers and caches the answer for next time.

**Negative caching**
The DNS cache also stores negative results, such as "this domain does not exist." This reduces load on authoritative servers and makes resolution more efficient.

### Types of DNS cache

**Client-side DNS cache**
- Computers, smartphones, and routers keep their own local DNS cache.
- It holds records for recently visited domains.
- This speeds up repeat lookups and browsing.

**DNS server cache**
- DNS servers, including recursive DNS servers run by internet service providers (ISPs) and public resolvers like Google Public DNS and OpenDNS, keep their own caches.
- They store results on behalf of many clients.
- This lowers resolution delay and improves overall network performance.

### Use cases and benefits

- **Faster DNS resolution:** no need to fetch mappings from external DNS servers every time.
- **Improved network performance:** less lookup delay, especially for sites you visit often.
- **Reduced DNS query traffic:** fewer queries reach authoritative servers, easing the load on DNS infrastructure.
- **Enhanced reliability:** cached mappings keep working even if authoritative servers are unreachable or down, so access to web services continues.

---

## Comparison of All Four Cache Types

| Feature | CPU cache | Disk cache | Web cache | DNS cache |
|---|---|---|---|---|
| **Stores** | Data and instructions | Disk data blocks | Web pages, images, media | Domain to IP mappings |
| **Location** | On the processor chip | In RAM | Browser, proxy, or CDN | Client device or DNS server |
| **Speeds up** | Memory access | Disk reads and writes | Page loading | Website lookups |
| **Levels or kinds** | L1, L2, L3 | OS, controller, software | Browser, proxy, CDN | Client-side, server |
| **Freshness control** | Coherency protocols | Write-back or write-through | Headers like ETag, Last-Modified | TTL |
| **Main benefit** | Faster execution | Faster I/O, less disk wear | Faster browsing, saved bandwidth | Faster lookups, reliability |

---

## Best Practices for Cache Management

Good cache management improves performance, keeps data correct, and avoids wasting resources.

### 1. Identify performance hotspots
Analyze your application to find where caching helps most. Focus on data that is accessed often, or on operations that need a lot of computing power.

### 2. Use caching strategically
Cache data that is read often but changes rarely. Avoid caching data that changes constantly, because it can become stale and cause inconsistent data.

### 3. Cache invalidation
Make sure cached data stays up to date. Common techniques:

| Technique | How it works |
|---|---|
| **Time-based expiration** | The entry expires after a set time (like TTL). |
| **Versioning** | A new version number tells the system to load fresh data. |
| **Event-driven invalidation** | When the underlying data changes, the cache entry is removed or refreshed. |

### 4. Eviction policies
Caches have limited space. When full, an eviction policy decides what to remove.

| Policy | What it removes |
|---|---|
| **LRU (Least Recently Used)** | The item not used for the longest time |
| **LFU (Least Frequently Used)** | The item used the fewest times |
| **TTL expiration** | Items that have passed their time limit |

Choose the policy that matches your application's access patterns and how long data should be kept.

### 5. Cache sizing
Size the cache based on available memory and expected workload.
- **Too large:** can cause memory pressure and more garbage collection overhead.
- **Too small:** causes frequent cache misses and weaker performance.

### 6. Fallback mechanisms
Handle cache misses gracefully. If data isn't in the cache, fall back to the real source (database, file system) to get it. Treat caching as a performance boost. The application must still work even if the cache is empty.

### 7. Security considerations
When caching sensitive or confidential data, protect it with access controls, encryption, and data masking, so it can't be accessed or exposed by unauthorized people.

---

## Important Points the Article Leaves Out

- **The hardest problem in caching is invalidation.** There is a famous saying: "There are only two hard things in computer science: cache invalidation and naming things." Serving old data by mistake is the most common cache bug.
- **Cache write strategies** go beyond disk cache. The article covers write-back and write-through. A third one, **write-around**, writes straight to the storage and skips the cache, so rarely read data doesn't fill it.
- **Cache-aside (lazy loading)** is the most common pattern in applications. The app checks the cache, and on a miss it loads from the database and then saves the result in the cache. This is what the "cache hit / cache miss" flow describes.
- **Application caches are missing from the article's list.** Tools like **Redis** and **Memcached** store data in memory so apps can skip slow database queries. They are among the most used caches in system design, and they link to your earlier topics on SQL vs. NoSQL (Redis is a key-value NoSQL store).
- **Cache hit ratio** is the key measure. It is the percentage of requests answered from the cache: hits divided by total requests. A high ratio means the cache is working well. A low ratio means you are caching the wrong things or the cache is too small.
- **Cache stampede (thundering herd).** If a popular item expires and thousands of requests miss at the same moment, they all hit the database at once and can crash it. Fixes include locking so only one request refills the cache, adding random time to expiry, or refreshing before it expires.
- **Cache warming.** A new or restarted cache is empty (a "cold cache"), so the first requests are slow. Warming means preloading popular data before users arrive.
- **Distributed caching.** When many servers share one cache layer, such as a Redis cluster, you must think about consistency, network delay, and what happens if a cache node fails.
- **HTTP caching headers** control web cache behavior. Beyond `Last-Modified` and `ETag`, there is `Cache-Control` (for example `max-age=3600` means keep for one hour) and `Expires`.
- **Stale DNS problems.** If a website changes its IP but your DNS cache holds the old one, you may reach the wrong server until the TTL expires. This is why DNS changes can take time to "propagate." You can clear your DNS cache manually if needed.
- **Security risks.** The article mentions protecting sensitive data. Two real attacks are **cache poisoning** (feeding false data into a cache, such as DNS cache poisoning that sends users to fake sites) and accidentally caching private pages so other users see them. Never cache personal or user-specific responses in a shared cache.
- **Trade-off: speed vs. freshness.** Every cache trades fresh data for speed. The longer you cache, the faster you are, but the more likely the data is out of date. Choose based on how costly stale data is.
- **Memory cost.** Caches use expensive, fast memory. You can't cache everything, which is why eviction policies exist.
- **Clearing cache fixes many bugs.** If a website looks broken or old after an update, clearing your browser cache is often the fix.
- **Other cache types exist** beyond the four listed, such as database query cache, application cache, API gateway cache, and reverse proxy cache (for example, NGINX or Varnish). This also connects to your earlier API gateway topic, since gateways can cache responses.

---

## Real-Life Examples

| Situation | Cache involved | What happens |
|---|---|---|
| You reopen a website and it loads instantly | Browser cache | Images and files come from your device |
| A video loads fast because the server is in your country | CDN cache | A nearby edge server delivers it |
| Typing a site name a second time finds it faster | DNS cache | The IP was already stored |
| Opening a recently used file is instant | Disk cache | The file data is already in RAM |
| A program runs smoothly during heavy work | CPU cache | Hot data stays in L1/L2/L3 |
| An app's product page loads fast under heavy traffic | Redis or similar | Database results are served from memory |

---

## When to Use Caching

### Use caching when
- The same data is read many times.
- The data changes rarely.
- Fetching or computing the data is slow or expensive.
- You need to reduce load on a database or server.
- You want faster response times for users.

### Be careful with caching when
- The data changes very often.
- Stale data would be harmful (account balances, stock levels, payments).
- The data is private or sensitive and the cache is shared.
- The data is rarely read, so it wastes cache space.

---

## Final Summary

| Question | Answer |
|---|---|
| What is a cache? | Fast, nearby storage for frequently used data |
| What is a cache hit? | Data found in the cache, so it is fast |
| What is a cache miss? | Data not found, so fetch from the slow source and store a copy |
| Cache inside the processor? | **CPU cache** (L1, L2, L3) |
| Cache that speeds up disk access? | **Disk cache** (uses RAM) |
| Cache for web pages and files? | **Web cache** (browser, proxy, CDN) |
| Cache for website address lookups? | **DNS cache** (uses TTL) |
| How do you keep cache data fresh? | Expiration, versioning, event-driven invalidation |
| What happens when the cache is full? | An eviction policy (LRU, LFU, TTL) removes items |
| What if data isn't in the cache? | Fall back to the original source |
| Biggest risk? | Serving stale data, and exposing sensitive data |
