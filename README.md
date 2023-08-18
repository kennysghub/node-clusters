# node-clusters
Utilizing clusters of Node.js processes to run multiple multiple application threads.



#### Without Node cluster module
Start server:
```
node index.js
```
Output:
```
App listening on port 3000
worker pid=4601
```
Run script to load test a URL: 
```bash
$ loadtest [-n requests] [-c concurrency] [-k] URL

$ npx loadtest -n 1200 -c 400 -k http://localhost:3000/heavy
```
Output:
```
Requests: 0 (0%), requests per second: 0, mean latency: 0 ms
Requests: 51 (4%), requests per second: 10, mean latency: 2363.2 ms
Requests: 98 (8%), requests per second: 9, mean latency: 7555.4 ms
Requests: 145 (12%), requests per second: 9, mean latency: 12504 ms
Requests: 193 (16%), requests per second: 10, mean latency: 17507.8 ms
Requests: 240 (20%), requests per second: 9, mean latency: 22527.3 ms
Requests: 287 (24%), requests per second: 9, mean latency: 27494.6 ms
Requests: 429 (36%), requests per second: 28, mean latency: 28121.6 ms
Errors: 142, accumulated errors: 142, 33.1% of total requests
Requests: 450 (38%), requests per second: 4, mean latency: 28575.3 ms
Errors: 13, accumulated errors: 155, 34.4% of total requests
Requests: 512 (43%), requests per second: 12, mean latency: 24989.2 ms
Errors: 15, accumulated errors: 170, 33.2% of total requests
Requests: 585 (49%), requests per second: 15, mean latency: 40253.7 ms
Errors: 34, accumulated errors: 204, 34.9% of total requests
Requests: 607 (51%), requests per second: 4, mean latency: 30650.5 ms
Errors: 0, accumulated errors: 204, 33.6% of total requests
Requests: 709 (59%), requests per second: 20, mean latency: 35773.2 ms
Errors: 91, accumulated errors: 295, 41.6% of total requests
Requests: 732 (61%), requests per second: 5, mean latency: 30002.9 ms
Errors: 23, accumulated errors: 318, 43.4% of total requests
Requests: 770 (64%), requests per second: 8, mean latency: 31900.7 ms
Errors: 1, accumulated errors: 319, 41.4% of total requests
Requests: 881 (73%), requests per second: 22, mean latency: 37623.5 ms
Errors: 111, accumulated errors: 430, 48.8% of total requests
Requests: 890 (74%), requests per second: 2, mean latency: 30832 ms
Errors: 6, accumulated errors: 436, 49% of total requests
Requests: 1001 (83%), requests per second: 22, mean latency: 34970.2 ms
Errors: 82, accumulated errors: 518, 51.7% of total requests
Requests: 1032 (86%), requests per second: 6, mean latency: 32486.6 ms
Errors: 31, accumulated errors: 549, 53.2% of total requests
Requests: 1048 (87%), requests per second: 3, mean latency: 32374.7 ms
Errors: 0, accumulated errors: 549, 52.4% of total requests
Requests: 1160 (97%), requests per second: 22, mean latency: 37591.6 ms
Errors: 92, accumulated errors: 641, 55.3% of total requests
Requests: 1160 (97%), requests per second: 0, mean latency: 0 ms
Errors: 0, accumulated errors: 641, 55.3% of total requests
Requests: 1190 (99%), requests per second: 6, mean latency: 38185.4 ms
Errors: 0, accumulated errors: 641, 53.9% of total requests

Target URL:          http://localhost:3000/heavy
Max requests:        1200
Concurrency level:   400
Agent:               keepalive

Completed requests:  1200
Total errors:        641
Total time:          110.98267 s
Requests per second: 11
Mean latency:        29310.1 ms

Percentage of the requests served within a certain time
  50%      30003 ms
  90%      41012 ms
  95%      41022 ms
  99%      49040 ms
 100%      49040 ms (longest request)

   -1:   641 errors
```
Almost half of the request resulted in errors.

#### With Node cluster module
Run `primary.js` to start server. H
```
$ node primary.js
```
Output:
```
The total number of CPUs is 10
Primary pid=5266
App listening on port 3000
worker pid=5267
App listening on port 3000
worker pid=5270
App listening on port 3000
worker pid=5272
App listening on port 3000
App listening on port 3000
worker pid=5276
worker pid=5273
App listening on port 3000
worker pid=5268
App listening on port 3000
worker pid=5271
App listening on port 3000
worker pid=5275
App listening on port 3000
worker pid=5269
App listening on port 3000
worker pid=5274
```
Run the same load-test script: 
```
$ npx loadtest -n 1200 -c 400 -k http://localhost:3000/heavy
```
Output:
```
Requests: 0 (0%), requests per second: 0, mean latency: 0 ms
Requests: 418 (35%), requests per second: 84, mean latency: 2320.2 ms
Requests: 808 (67%), requests per second: 78, mean latency: 2697.2 ms
Errors: 9, accumulated errors: 9, 1.1% of total requests

Target URL:          http://localhost:3000/heavy
Max requests:        1200
Concurrency level:   400
Agent:               keepalive

Completed requests:  1200
Total errors:        9
Total time:          15.006753167000001 s
Requests per second: 80
Mean latency:        4158.3 ms

Percentage of the requests served within a certain time
  50%      3950 ms
  90%      8508 ms
  95%      8528 ms
  99%      8587 ms
 100%      8716 ms (longest request)

   -1:   9 errors
```