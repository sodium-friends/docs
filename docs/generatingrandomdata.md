---
id: generatingrandomdata
title: Generating Random Data
sidebar_label: Generating Random Data
---

Bindings for the random data generation API. [See the libsodium randombytes docs for more information](https://download.libsodium.org/doc/generating_random_data/).

## Constants
**Buffer lengths (integer)**
* `randombytes_SEEDBYTES`

***
## `randombytes_random`
![sodium-native][node]
``` js
var uint32 = sodium.randombytes_random() 
```
Generates a random 32-bit unsigned integer `[0, 0xffffffff]` (both inclusive).
***
## `randombytes_uniform`
![sodium-native][node]
``` js
var uint = sodium.randombytes_uniform(upper_bound)
```
Generates a random 32-bit unsigned integer `[0, upper_bound)` (last exclusive).
* `upper_bound` must be at most `0xffffffff`
***
## `randombytes_buf`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.randombytes_buf(buffer)
```
Fills `buffer` with random data.
***
## `randombytes_buf_deterministic` 
![sodium-native][node]
``` js
sodium.randombytes_buf_deterministic(buffer, seed)
```
Fills `buffer` with random data, generated from `seed`.
* `seed` must be a `buffer` of length at least `randombytes_SEEDBYTES`


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
