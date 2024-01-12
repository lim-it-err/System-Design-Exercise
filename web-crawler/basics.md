# Web-Crawlers
## Definition / Usage of Web Crawlers.
### Definition
- Web Crawlers is an internet bot for finding systematically browses the content of World Wide Web.

### Usage
- Serach Engine Indexing (i.e. Google Search Engine)
- Web archiving
- Web mining
- Web monitoring


## Building User Requirements
### Collecting User's needs with Assumptions
```text

Primary Use: The crawler's main purpose is for search indexing.

Data Volume: The objective is to gather approximately 1 billion web pages each month. Given the web's extensive size (around 60 trillion pages), this pace would require about 30 years to cover the entire web.

Regular Updates: The crawler should efficiently identify and index web pages that are newly created or have undergone modifications.

Long-Term Storage: There is a requirement to store the collected web pages for a duration of 5 years, emphasizing the elimination of duplicate content.

Crawl Priority: Collection of pages will be conducted without any specific priority.

Multi-Language: Language variations do not need to be a consideration in the crawling process.
```
### Additional Assumptions from the Developer's Perspective
```text

Legal Considerations: Given the hypothetical nature of this exercise, legal aspects are not a concern.

Robustness: The crawler must be resilient against potential threats such as malware, unresponsive servers, and flawed codes.

Politeness: The crawler should avoid sending excessive requests to the same page in a short timeframe.

Scalability: The code should be optimized for high performance, including capabilities for parallel processing.

Extensibility: The code structure must be adaptable to accommodate a variety of evolving needs and potential modifications.
```

## Infra
### Estimation 
QPS
|Index| Content                                                           |
|---|-------------------------------------------------------------------|
|QPS(read)| 1 billion / 30 days / 24 hours / 3600 seconds ~= 400 pages/second |
|Maximum QPS(read)| 2 *480 pages/second = 960 pages/second|
|QPS(write) | Same as QPS(read), or (1-duplicated_ratio)*QPS(read)              |

Storage

In-Memory

GPU

Database Read/Write

Network Bandwidth

## Architecture
### Cluster Architecture
### Software Architecture
Structure Diagram

Behaviour Diagram

## In detaiils
### Algorithm
1. Graph Traversal
2. Managing problematic Contents
3. Detecting Duplicated Contents

### Database Schema
### Optimization
Performance
1. Distributed 
2. Caching
3. Locality
4. Timeout

Durability
1. Hashing
2. Memorization
3. Solid Exception


## Further Talk
### Analyzation Tool
### Link with Machine Learning
## Reference
