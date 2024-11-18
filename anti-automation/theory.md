1. Resource exhaustion through improper control of interaction frequency :-
   TESTCASES :-
   1. Resource exhaustion due to lack of automation in file upload.
   2. check for request with big response size 
      ex :- /api/users?page=1&size=100 :- let this api fetches 100 requests 
            but let attacker changes the size to size=2000000 causing performance issue on db and 
            disrupting the availability of response to any other user leading to resource exhaustion.
   3. In burp suite logger++ extension check for uncommonly large round trip time (RTT) 
   4. known intensive processing (e.g. sql or nosql queries with no LIMIT, Graphql requests with no rate or response limiting).


2. RATE LIMITING :-
   Most prominently rate limiting is used to economize server-side resources, ensure stable service for all users, 
   and limit the risk of cascading failure of connected systems. Other reasons to implement rate limiting might be
   limiting costs for automatically scaling systems or directing work flows to workers. There are different approaches
   to rate limiting, but it is usually performed by assigning each user of a system a contingent of actions for a certain timeframe.

   Rate limiting and load balancing usually go hand in hand and most application-level load balancers can also be used 
   as a rate limiter.

   Rate limiting headers :- 
   429 Too Many Requests status code for HTTP and the Retry-After header field which should be returned once a rate limit is reached.

   The draft defines three headers for rate-limited responses:

    RateLimit-Limit: containing the requests quota in the time window;
    RateLimit-Remaining: containing the remaining requests quota in the current window;
    RateLimit-Reset: containing the time remaining in the current window, specified in seconds or as a timestamp;

    Some applications might still implement experimental versions of the same, where commonly used header field names are (ref):
    X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset;
    X-Rate-Limit-Limit, X-Rate-Limit-Remaining, X-Rate-Limit-Reset.

    There are variants too, where the window is specified in the header field name, eg:
    x-ratelimit-limit-minute, x-ratelimit-limit-hour, x-ratelimit-limit-day
    x-ratelimit-remaining-minute, x-ratelimit-remaining-hour, x-ratelimit-remaining-day
    Those variants should not be used in newer deployments as the “X-“ prefix and similar constructs are deprecated by RFC-6648 (ref).

    Rate limiting can be implemented on different platfroms :- aws, azure, gcp , cloudflare, ngix

    Consequences
    In summary, once a rate limit threshold request is reached, further requests are blocked with a customizable 429 message, 
    with a retry-after header included. Cached pages are excluded from rate limiting, and this does not affect SEO. 429s can be 
    returned from the source server as well if the source server has its own rate limiting. The rate limiting affects a single IPv4 or IPv6.

    Notes :- 
    Cached resources and known Search Engine crawlers are exempted from a customer's Rate Limiting rules. 
    Rate Limiting does not negatively affect a website’s SEO Ranking. 
    Once an individual source (IP or IPv6) exceeds a rule threshold, further requests to the origin web server are blocked with an HTTP 429 response.
    A Retry-After header is included to indicate when the client can resume sending requests.
    HTTP 429 includes 429 responses returned from the origin if the origin web server also applies its own rate limiting.

    