# Rate Limiter (Sliding Window Log) - LLD Problem Statement

## Problem Description

Design and implement a rate limiter that restricts the number of requests a client can make within a configurable time window.

The rate limiter should support multiple clients independently and ensure that each client is allowed to make at most **N requests** within the last **W seconds**.

If accepting a new request would cause the client's total requests within the active window to exceed the configured limit, the request should be rejected.

---

## Functional Requirements

1. The system should support multiple clients identified by a unique `clientId`.
2. Each client has an independent rate limit.
3. A request contains:

   * Client Identifier
   * Number of requests being consumed
   * Timestamp
4. Requests older than the configured time window should no longer be considered for rate limiting.
5. The system should expose the following API:

```java
boolean isAllowed(String clientId, int requests)
```

where:

* `clientId` identifies the client making the request.
* `requests` represents the number of request units being consumed.

The method should:

* Return `true` if the request can be accepted.
* Return `false` if accepting the request would exceed the configured limit.

---

## Example

Configuration:

```text
Limit = 100 requests
Window = 60 seconds
```

Timeline:

```text
t = 0s   -> Client A consumes 20 requests
t = 10s  -> Client A consumes 30 requests
t = 20s  -> Client A consumes 40 requests
```

Current usage:

```text
20 + 30 + 40 = 90 requests
```

At:

```text
t = 30s
```

Request:

```java
isAllowed("A", 15)
```

Result:

```text
false
```

because:

```text
90 + 15 = 105 > 100
```

---

## Assumptions

1. Requests arrive in chronological order.
2. Time is represented using epoch milliseconds.
3. This version focuses on a single-node, in-memory implementation.
4. Distributed rate limiting is out of scope.
5. Persistence is not required.

---

## Expected Solution

Implement the rate limiter using the Sliding Window Log approach:

* Maintain request history for each client.
* Remove expired requests from the active window.
* Maintain a running count of requests within the window.
* Allow or reject requests based on the configured limit.

---

## Complexity Targets

### Time Complexity

* Request validation: O(1) amortized
* Request insertion: O(1)

### Space Complexity

* O(active requests within the current window)

---

# Single thread solution with sliding window

```java
package lldproblems;


import java.util.ArrayDeque;
import java.util.Map;
import java.util.Queue;
import java.util.concurrent.ConcurrentHashMap;

class RateLimiterRequest
{
    private final int requests;
    private final long ts;

    public RateLimiterRequest(int requests, long ts) {
        this.requests = requests;
        this.ts = ts;
    }

    public long getTs() {
        return ts;
    }


    public int getRequests() {
        return requests;
    }
}

public class RateLimiter {

    private final int requestsLimit;
    private final int windowSeconds;
    private Map<String, Queue<RateLimiterRequest>> map;
    private Map<String, Long> countMap;

    public RateLimiter(int requestsLimit, int windowSeconds) {
        this.requestsLimit = requestsLimit;
        this.windowSeconds = windowSeconds;
        map = new ConcurrentHashMap<>();
        countMap = new ConcurrentHashMap<>();
    }



    boolean isAllowed(String clientId, int requests)
    {

        if(!map.containsKey(clientId))
        {
            map.put(clientId, new ArrayDeque<>());
            countMap.put(clientId, 0L);
        }

        removeNodesBeyondWindow(clientId);

        if(countMap.get(clientId) + requests > requestsLimit)
            return false;

        RateLimiterRequest obj = new RateLimiterRequest(requests, getCurrentTimeStamp());

        Queue<RateLimiterRequest> q = map.get(clientId);
        q.add(obj);

        countMap.put(clientId, countMap.get(clientId) + obj.getRequests());
        return true;
    }

    void removeNodesBeyondWindow(String clientId)
    {
        Queue<RateLimiterRequest> q = map.get(clientId);
        long now = getCurrentTimeStamp();

        while(!q.isEmpty() && now - q.peek().getTs() > windowSeconds*1000)
        {
            RateLimiterRequest obj = q.poll();
            countMap.put(clientId, countMap.get(clientId)-obj.getRequests());
        }
    }

    long getCurrentTimeStamp()
    {
        //logic to return currentTimestamp;

        return System.currentTimeMillis();
    }

}

```