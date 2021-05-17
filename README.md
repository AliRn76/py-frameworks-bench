# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-05-17

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-05-17)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B557115%2C490275%2C466005%2C386505%2C355890%2C329745%2C282915%2C222930%2C132060%2C116940%2C66675%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-05-17)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.5` | 19800 | 3.33 | 4.06 | 3.19
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 17226 | 3.83 | 4.68 | 3.67
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 16558 | 4.09 | 4.88 | 3.83
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14658 | 4.67 | 5.53 | 4.33
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 14528 | 4.71 | 5.58 | 4.37
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 9953 | 6.84 | 8.26 | 6.39
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 9466 | 7.35 | 8.61 | 6.76
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7718 | 8.29 | 8.72 | 8.30
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3588 | 17.71 | 18.54 | 17.83
| [quart](https://pypi.org/project/quart/) `0.15.0` | 3565 | 18.10 | 19.24 | 17.94
| [django](https://pypi.org/project/django/) `3.2.3` | 1840 | 34.69 | 38.33 | 34.78


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.5` | 6185 | 11.30 | 13.50 | 10.34
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 4642 | 15.02 | 18.06 | 13.75
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 4570 | 15.91 | 17.90 | 13.97
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3961 | 17.77 | 20.57 | 16.21
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2606 | 27.90 | 30.76 | 24.51
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2375 | 26.93 | 27.69 | 26.93
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 2336 | 30.85 | 33.51 | 27.31
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2288 | 27.82 | 28.75 | 27.99
| [quart](https://pypi.org/project/quart/) `0.15.0` | 1951 | 32.17 | 33.59 | 32.78
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1588 | 37.57 | 45.62 | 40.26
| [django](https://pypi.org/project/django/) `3.2.3` | 1010 | 62.88 | 70.86 | 63.24


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.5` | 11156 | 6.22 | 7.37 | 5.70
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 10817 | 6.08 | 7.63 | 5.88
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10548 | 6.35 | 7.86 | 6.03
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 8503 | 8.38 | 9.70 | 7.49
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7947 | 8.82 | 10.29 | 8.05
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 7610 | 9.25 | 10.75 | 8.46
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 6572 | 10.59 | 12.61 | 9.71
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4769 | 13.42 | 13.97 | 13.42
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2928 | 21.75 | 22.60 | 21.87
| [quart](https://pypi.org/project/quart/) `0.15.0` | 2280 | 27.46 | 28.72 | 28.05
| [django](https://pypi.org/project/django/) `3.2.3` | 1595 | 40.85 | 44.13 | 40.07

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.5` | 557115 | 6.95 | 8.31 | 6.41
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 490275 | 8.31 | 10.12 | 7.77
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 466005 | 9.4 | 11.1 | 8.69
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 386505 | 13.65 | 15.33 | 12.11
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 355890 | 17.18 | 20.65 | 17.7
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 329745 | 10.69 | 12.27 | 9.59
| [fastapi](https://pypi.org/project/fastapi/) `0.65.1` | 282915 | 16.09 | 18.13 | 14.47
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 222930 | 16.21 | 16.79 | 16.22
| [tornado](https://pypi.org/project/tornado/) `6.1` | 132060 | 22.43 | 23.3 | 22.56
| [quart](https://pypi.org/project/quart/) `0.15.0` | 116940 | 25.91 | 27.18 | 26.26
| [django](https://pypi.org/project/django/) `3.2.3` | 66675 | 46.14 | 51.11 | 46.03

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)