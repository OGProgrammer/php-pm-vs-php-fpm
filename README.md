## PHP-PM vs PHP-FPM

This is a laravel 7.12 example using both PHP-PM and PHP-FPM side by side to benchmark.

## MacOSX
 Prerequisites 
* Install docker/docker-compose
* Install siege with `brew install siege` or `brew install ab`

How to run this stuff
* To shut this down, run `docker-compose down`
* To bring it up, run `docker-compose up -d --build`
* To tail docker logs `docker-compose logs -f`
* To tail laravel logs run `tail -f storage/logs/*`
* To benchmark the PHP-FPM container, run `ab -c10 -n1000 http://127.0.0.1:8888/` or `siege -b -t30S http://localhost:8888`
* To benchmark the PHP-PM container, run `ab -c10 -n1000 http://127.0.0.1:8080/` or `siege -b -t30S http://localhost:8080`

### Important files to checkout
* server.conf
* docker-compose.yml


#### My benchmarks

Using `ab -c10 -n1000`
```

# PHP-PM
Concurrency Level:      10
Time taken for tests:   85.509 seconds
Complete requests:      1000
Failed requests:        17
   (Connect: 0, Receive: 0, Length: 17, Exceptions: 0)
Non-2xx responses:      17
Total transferred:      3267703 bytes
HTML transferred:       2385285 bytes
Requests per second:    11.69 [#/sec] (mean)
Time per request:       855.089 [ms] (mean)
Time per request:       85.509 [ms] (mean, across all concurrent requests)
Transfer rate:          37.32 [Kbytes/sec] received

# PHP-FPM
Concurrency Level:      10
Time taken for tests:   160.549 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      3393000 bytes
HTML transferred:       2426000 bytes
Requests per second:    6.23 [#/sec] (mean)
Time per request:       1605.488 [ms] (mean)
Time per request:       160.549 [ms] (mean, across all concurrent requests)
Transfer rate:          20.64 [Kbytes/sec] received

```

Using `siege -b -t30S`
```
# PHP-PM
{       "transactions":                          794,
        "availability":                        98.15,
        "elapsed_time":                        29.89,
        "data_transferred":                     1.10,
        "response_time":                        0.71,
        "transaction_rate":                    26.56,
        "throughput":                           0.04,
        "concurrency":                         18.95,
        "successful_transactions":               794,
        "failed_transactions":                    15,
        "longest_transaction":                 16.08,
        "shortest_transaction":                 0.09
}

# PHP-FPM
{       "transactions":                          260,
        "availability":                       100.00,
        "elapsed_time":                        29.21,
        "data_transferred":                     0.16,
        "response_time":                        1.54,
        "transaction_rate":                     8.90,
        "throughput":                           0.01,
        "concurrency":                         13.72,
        "successful_transactions":               260,
        "failed_transactions":                     0,
        "longest_transaction":                  6.19,
        "shortest_transaction":                 0.10
}
```
### Other notes

Not sure why there are some failed tx but probably from siege not waiting?
