# Short-URL

## What is short-url?
- Generate URL within limited Length
- Map Short-url to original, longer URL

## Building Requirements/ Non-Requirements by Assumptions
`**` is considered to be changed in near_future(hint for next assumptions?)
You don't have to consider for `*`.

### Assumptions
Traffic

    1. Write: 100 million per day
    2. Read:Write = 20:1
    3. Maximum QPS = 2
    4. All Traffic are served in domestic **
    
Server Capabilities

    1. Every network is connected by Gigabit speed
    2. RAM will be around 256GB ~ 512GB.
    3. Every other factor will be similar as M1 Pro, for experiment
  

## Requirements
### Functional-Requirements
```
1. User could generate short-url by long-url
2. When user requests short-url, long-url should be redirected after generation.
3. Short URL should be in brief length as possible
4. User could not customize short-url **

5. You don't have to analyze the service pattern. Of course, you may! **
6. Short-url should remain unchanged.
7. Single short-url should match by 1:1 with long-url
8. lifespan for short-url is 10 years. You don't have to delete/renew **
```
### Non-functional Requirements
```
1. Service has to be run every time. (error-prune) 
2. Service should ensure low latency on redirecting short-url
3. Service should be accessible by REST-API for user
4. Service should be capable of managing high-traffic
```

## Solve
### Calculate Read-Write frequency
|Index|Content|
|---|---|
|DAU(write), Daily Active User|100 million|
|QPS(write), Query Per Second |1200|
|QPS(read)|1200*20 = 24000|
|Maximum QPS|24000 * 2 = 48000|

- write/sec: 100 million / 24(hours)/60(minutes)/60(seconds) = 1157 ~=1200
- QPS(read/sec): 1200 * 20 =24000
- Maximum QPS: 24000 * 2(Maximum QPS) = 48000

### Assume Database Capabilites
- After selecting database, I will take experiment on transaction speed.
  - Database transaction speed differs by `DBMS Selection`, `Lock Mechanism`, `Transaction Level` ...
  - We will look on it later.
- I have assumed DB transaction speed as 25k. (will experiment in near future)
  - We need distribution at least 2 databases. 

### Storage
WIP

### Reference
- https://systemdesign.one/url-shortening-system-design/
