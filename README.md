## PHP-PM vs PHP-FPM

This is a laravel 7.12 example using both PHP-PM and PHP-FPM side by side to benchmark.

## MacOSX
 Prerequisites 
* Install docker/docker-compose
* Install siege with `brew install siege`

How to run this stuff
* To shut this down, run `docker-compose down`
* To bring it up, run `docker-compose up -d --build`
* To tail docker logs `docker-compose logs -f`
* To tail laravel logs run `tail -f storage/logs/*`
* To benchmark the PHP-FPM container, run `siege -b -t30S http://localhost:8888`
* To benchmark the PHP-PM container, run `siege -b -t30S http://localhost:8080`

### Important files to checkout
* server.conf
* docker-compose.yml


#### My benchmarks
Using `siege -b -t30S`
```
# PHP-PM

{       "transactions":                          707,
        "availability":                        97.12,
        "elapsed_time":                        29.02,
        "data_transferred":                     0.98,
        "response_time":                        0.96,
        "transaction_rate":                    24.36,
        "throughput":                           0.03,
        "concurrency":                         23.41,
        "successful_transactions":               707,
        "failed_transactions":                    21,
        "longest_transaction":                 27.00,
        "shortest_transaction":                 0.03
}


# PHP-FPM
{       "transactions":                          458,
        "availability":                       100.00,
        "elapsed_time":                        29.60,
        "data_transferred":                     0.28,
        "response_time":                        1.49,
        "transaction_rate":                    15.47,
        "throughput":                           0.01,
        "concurrency":                         23.05,
        "successful_transactions":               458,
        "failed_transactions":                     0,
        "longest_transaction":                 22.57,
        "shortest_transaction":                 0.09
}

```
