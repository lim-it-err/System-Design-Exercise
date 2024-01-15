# Web-Crawlers
## Definition / Usage of Web Crawlers.
### Definition
- Web Crawlers is an internet bot for finding systematically browses the content of World Wide Web.

### Usage
- Serach Engine Indexing (i.e. Google Search Engine)
- Web archiving
- Web mining
- Web monitoring

We will look into Search Engine Indexing

## Building User Requirements
### Collecting User's needs with Assumptions
```text

Purpose: The crawler's main purpose is for search indexing.

Data Volume: The objective is to gather approximately 1 billion web pages each month. 
Given the web's extensive size (around 60 trillion pages), this pace would require about 30 years to cover the entire web.

Regular Updates: The crawler should efficiently identify and index web pages that are newly created or have undergone modifications.

Long-Term Storage: store the collected web pages for a duration of 5 years, emphasizing the elimination of duplicate content.

Multi-Language: Language variations do not need to be a consideration in the crawling process.
```
### Additional Assumptions from the Developer's Perspective
```text
Legal Considerations: Given the hypothetical nature of this exercise, legal aspects are not a concern.

Robustness: The crawler must be resilient against potential threats such as malware, unresponsive servers, and flawed codes.

Politeness: The crawler should avoid sending excessive requests to the same page in a short timeframe.

High-Performance: The code should be optimized for high performance, including capabilities for parallel processing.

Scability: Since traffic is consistent, we don't need to care of scability.

Cost : Concern Hardware Architecture be cheap.

Extensibility: The code structure must be adaptable to accommodate a variety of evolving needs and potential modifications.
```

## Infra
### Estimation 
QPS
|Index| Content                                                           |
|---|-------------------------------------------------------------------|
|QPS(read)| 1 billion / 30 days / 24 hours / 3600 seconds ~= 400 pages/second |
|Maximum QPS(read)| 2 *400 pages/second = 800 pages/second |
|QPS(write) | Same as QPS(read), or (1-duplicated_ratio)*QPS(read)             |

Storage
- 1 billion/month * 500kb/pages * 12 month * 30 years = 4800 billion KB = 120000 TB
    ![스크린샷 2024-01-15 오후 9.33.31.png](..%2F..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2F7w%2F_w36dp4s1j5cxhz3khwpsxc40000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_tI1btE%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-01-15%20%EC%98%A4%ED%9B%84%209.33.31.png)
  (ChatGPT's answer)
    - Consider that single Magnetic Tape ~= 200 TB
    - Also, We need consideration of Write I/O.

In-Memory 
- No In-memory space.
- We may use it for cache, but not necessary.

GPU
- No GPU being used, for now.
- I/O Bound > CPU Bound Task

Database Read/Write
- find page list: 400 ops/second
- write page list : 400 ops/second

Network Bandwidth
- 800 pages * 500kb  = 800 pages * 500kb * 8bit * (kb/1000bit)*(mb/1000kb) = 50 Mbps

Conclusion
- `Constructing Large Storage` is the most problem
- Not concern of `network`, `Database I/O` on current status. 
- High concern of `response time`

## Architecture
### Components
1. URL Repository
   - Feature
      - List of URLs to be traversed (URLs to visit and update) 
        - URL to be traversed
        - URL to be updated (with update policy. We cannot traverse all.)
      - Maintain a list of URLs in memory
      - Caching mechanism for faster updates (with caching policy)
   - Considerations
     - Graph Traversal Considerations
     - Data Type: List? Tree? B+ Tree?
     - Database Type : NoSQL? RDBMS?

2. HTML Downloader
   - Feature
      - Download HTML files from web pages
      - Requires DNS Resolver for hostname resolution
      - Asynchronous operation for network-bound tasks
   - Considerations
     - How to manage Async Tasks 
3. HTML File Validator
   - CPU-bound task (can utilize multithreading or distributed processing)
   - Can be implemented in parallel for multiple HTML files

4. Duplicate HTML Checker
   - Highly dependent on a database (read operations)
   - Parallel implementation possible
   - Asynchronous operation for efficient processing

5. URL Extractor
   - Extract URLs from HTML files
   - Can be integrated with the Duplicate HTML Checker

6. URL Filter & Manager
   - Check URL duplicity and manage the URL Repository

7. RSS Feed Analyzer (optional)
   - Analyze RSS feeds if needed for content updates

    
Abstract Diagram
```text

Components Interaction:
URL Repository - HTML Downloader            - HTML File Validator 
               |
               Message Queue
               |
               |- Duplicate HTML Checker    - URL Extractor
               |                            |
               |                            |- URL Filter & Manager
               |- RSS Feed Analyzer (if RSS feeds are considered)
```


### Cluster Architecture
- Considerations
  - SPOF(Single Point of Fault)
  - I/O Bounds (Database, Internet connection)
  - Database
  WIP

### Software Architecture
- Structure Diagram
  - WIP
- Behaviour Diagram
  - WIP
## In detaiils
### Algorithm
1. Graph Traversal Considerations
   - DFS 
     - Critical when large depth
     - Hard for Parallel Computing
   - BFS
     - Impolite Crawler Traverse similar-links in order
       - Make queue by domain (Lock single domain)
   - Both
     - Priority: Need to be prioritized based on user's choice
        - Based on Priority Queue
     
2. Managing problematic Contents
3. Detecting Duplicated Contents
### Respecting `Robots.txt`
### Database Schema
WIP
### Optimization
Performance
1. Distributed 
    - Treated on above
2. Caching
    - Caching URL Repository
      - 8Gb / 1Kb = 8e6 (pages)
    - Caching Mapping Table(Back-Q)
3. Locality
    - Locate Cache Server on every region
      - Does region matter on ISP?
      - or region matter on Routing Table?
4. Timeout
    - Short Timeout

Durability
1. Hashing
2. Memorization
3. Solid Exception


## Further Talk
### Analyzation Tool
### Link with Machine Learning
## Reference
