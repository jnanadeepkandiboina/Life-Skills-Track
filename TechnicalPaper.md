**Technical Paper on Caching**
===

Recently I joined a new project, and the team is facing some performance and scaling issues. Some features are becoming slow
when trafficincreases, and the database is getting too many calls. After checking a few things, my team lead asked me to look
into different caching approaches and understand how caching can help solve these problems.
Below is what I understood about caching in a simple way.

**What is Caching?**
---

Caching basically means storing some data temporarily so that we don't have to calculate it again or fetch it again from the 
database. If the same request comes again, we just pick it from the cache, which is much faster.

So overall caching helps in:
- Reducing load on the database
- Faster responses
- Better performance when many users come at the same time
- Saving time and resources

**Types of Caching I Learned**
===

**1. In-Memory Cache**
---

This is the simplest type.
The data is stored in the application’s memory itself. For example, using a HashMap in Java.

Pros:
- Very fast
- Easy to use

Cons:
- Works only on one server
- Data gets deleted if the server restarts
- Good for storing small things like configuration or data that rarely changes.

**2. Distributed Cache**
---

Here the cache is outside the application, usually in tools like Redis or Memcached.
All application servers can access the same cache.

Pros:
- Works well in projects with multiple servers
- Data stays even if one server goes down

Cons:
- Needs extra setup
- Slightly slower than in-memory because of network calls
- Most modern systems use Redis because it is fast and reliable.

**3. Database Query Caching**
---

Sometimes the problem is that the database is getting too many read queries.
If the same query is asked again and again, we can store its result in cache.

Pros:
- Reduces database load
- Good for read-heavy operations

Cons:
- Need to handle stale data when something is updated
- This is useful for dashboards or pages that don't change often.

**4. HTTP or CDN Caching**
---

This type caches complete HTTP responses or static files like images, CSS, JS etc.
Tools like Cloudflare or Varnish do this.

Good for:
- Static files
- Public pages
- Reducing load on backend servers

**5. Application-Level Caching**
---

Sometimes the application does expensive calculations.
If the result can be reused later, we can store it in cache.

Good for:
- Data processing
- Any function that takes a lot of time

**Cache Invalidation**
---

One problem with caching is that cached data can become old or wrong.

There are some ways to handle this:
- TTL (Time To Live): Data expires after some time
- Manual clear: We clear the cache when something changes
- Cache-aside: First check cache → if not found → fetch from database → store in cache
Cache-aside is the most common and easiest to use.

**What I Recommend for Our Project**
---

Based on the issues we are facing, I think using a distributed cache like Redis will help the most.
We can cache the most repeated database queries, and also add proper expiry times so the data doesn’t become too old.
Also, static content can be cached at the front end using browser or CDN caching.
This will reduce load on the database and also make the whole system faster.

**Conclusion**
---

Caching is a simple but very powerful way to improve the performance of an application.
By storing frequently used data in a faster place, we can avoid unnecessary work.
For our project, using Redis and caching some repeated queries should help us solve many performance and scaling problems.

**References**
---

 - [Caching - System Design Concept](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.geeksforgeeks.org/system-design/caching-system-design-concept-for-beginners/&ved=2ahUKEwi95fK4gJeRAxU-S2wGHctJCTYQFnoECBcQAQ&usg=AOvVaw1cvkhd7YFOyeLcQM7XHim6)
 - [What is Caching and How it Works](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://aws.amazon.com/caching/&ved=2ahUKEwi95fK4gJeRAxU-S2wGHctJCTYQFnoECGQQAQ&usg=AOvVaw1hsdvXFxFoKBEwmQMx0CU-)
 - [What is caching? | How is a website cached?](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.cloudflare.com/learning/cdn/what-is-caching/&ved=2ahUKEwi95fK4gJeRAxU-S2wGHctJCTYQFnoECE4QAQ&usg=AOvVaw2VkVVojCtuW8tu9muxxGUi)
