# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-06-14

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
* [The Results](#the-results-2021-06-14)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B538695%2C486690%2C452850%2C372375%2C346680%2C324330%2C281490%2C227865%2C130545%2C114555%2C72525%5D%7D%5D%7D%7D' />

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


## The Results (2021-06-14)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.7` | 19097 | 2.76 | 4.32 | 3.31
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 17129 | 3.08 | 4.85 | 3.70
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 15966 | 3.28 | 5.22 | 3.97
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 14029 | 3.72 | 5.96 | 4.53
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14026 | 3.64 | 5.96 | 4.53
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 10014 | 5.04 | 8.41 | 6.36
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 9330 | 5.29 | 8.97 | 6.86
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7869 | 8.08 | 8.25 | 8.13
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3520 | 18.88 | 19.52 | 18.16
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3472 | 18.42 | 18.57 | 18.43
| [django](https://pypi.org/project/django/) `3.2.4` | 2036 | 31.48 | 35.03 | 31.48


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.7` | 10681 | 4.63 | 7.97 | 5.95
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 10645 | 4.63 | 8.03 | 5.98
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10399 | 4.79 | 8.21 | 6.14
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 8209 | 6.03 | 10.50 | 7.76
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7843 | 6.28 | 10.76 | 8.15
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 7456 | 6.61 | 11.20 | 8.62
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 6447 | 7.60 | 13.34 | 9.89
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4854 | 13.11 | 13.30 | 13.18
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2937 | 21.77 | 21.94 | 21.79
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2223 | 28.46 | 28.99 | 28.77
| [django](https://pypi.org/project/django/) `3.2.4` | 1708 | 39.12 | 41.10 | 37.44

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.7` | 6135 | 8.02 | 14.05 | 10.45
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 4672 | 10.52 | 18.58 | 13.72
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 4449 | 11.04 | 19.35 | 14.41
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3825 | 13.56 | 21.96 | 16.77
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2590 | 20.75 | 32.66 | 24.67
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2468 | 25.91 | 26.00 | 25.92
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 2305 | 32.90 | 36.64 | 27.72
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2294 | 27.90 | 27.95 | 27.89
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1894 | 33.69 | 34.27 | 33.78
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 1627 | 36.25 | 44.98 | 39.30
| [django](https://pypi.org/project/django/) `3.2.4` | 1091 | 58.60 | 65.08 | 58.55


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.7` | 538695 | 5.14 | 8.78 | 6.57
| [muffin](https://pypi.org/project/muffin/) `0.79.1` | 486690 | 6.08 | 10.49 | 7.8
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 452850 | 7.21 | 11.8 | 8.96
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 372375 | 10.14 | 16.37 | 12.32
| [emmett](https://pypi.org/project/emmett/) `2.2.2` | 346680 | 15.53 | 20.71 | 17.48
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 324330 | 7.54 | 13.03 | 9.81
| [fastapi](https://pypi.org/project/fastapi/) `0.65.2` | 281490 | 15.18 | 19.46 | 14.66
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 227865 | 15.7 | 15.85 | 15.74
| [tornado](https://pypi.org/project/tornado/) `6.1` | 130545 | 22.7 | 22.82 | 22.7
| [quart](https://pypi.org/project/quart/) `0.15.1` | 114555 | 27.01 | 27.59 | 26.9
| [django](https://pypi.org/project/django/) `3.2.4` | 72525 | 43.07 | 47.07 | 42.49

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)