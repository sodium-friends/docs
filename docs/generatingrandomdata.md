---
id: generatingrandomdata
title: Generating Random Data
sidebar_label: Generating Random Data
---

Bindings to the random data generation API. [See the libsodium randombytes docs for more information](https://download.libsodium.org/doc/generating_random_data/).
***
## `randombytes_random`
![sodium-node][node]
``` js
var uint32 = sodium.randombytes_random() 
```
Generate a random 32-bit unsigned integer `[0, 0xffffffff]` (both inclusive).
***
## `randombytes_uniform`
![sodium-node][node]
``` js
var uint = sodium.randombytes_uniform(upper_bound)
```
Generate a random 32-bit unsigned integer `[0, upper_bound)` (last exclusive).
* `upper_bound` must be `0xffffffff` at most
***
## `randombytes_buf`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.randombytes_buf(buffer)
```
Fill `buffer` with random data.
***
## `randombytes_buf_deterministic` 
![sodium-node][node]
``` js
sodium.randombytes_buf_deterministic(buffer, seed)
```
Fill `buffer` with random data, generated from `seed`.
* `seed` must be a `buffer` of length at least `sodium.randombytes_SEEDBYTES`


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
