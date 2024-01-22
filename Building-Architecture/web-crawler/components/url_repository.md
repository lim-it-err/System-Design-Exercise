# URL Repository
## Concept
### Role
- Make List/Tree for candidate of URL.
  - Exclude if URL are already visited from database
- Prioritize URLs
- Make similar URL to be visited separately.

### Input
Given URL / candidate/url from Redis
- Stored by `Key`:`Value` = `URL`: `candidate url`

### Output
Kafka by multiple topic, separated by priority and revisit period

### Database
- Any RDBMS Database
## Content
### Why Redis?
- After parsing from each cluster, each value should be managed by key
- Each value could come from cluster, so managing each connection might be hard(i.e. websocket)
