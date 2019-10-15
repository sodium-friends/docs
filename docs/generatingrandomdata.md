---
id: generatingrandomdata
title: Generating random data
sidebar_label: Generating random data
---

Bindings to the random data generation API. [See the libsodium randombytes docs for more information](https://download.libsodium.org/doc/generating_random_data/).

``` js
var uint32 = sodium.randombytes_random()
```
Generate a random 32-bit unsigned integer `[0, 0xffffffff]` (both inclusive).

``` js
var uint = sodium.randombytes_uniform(upper_bound)
```
Generate a random 32-bit unsigned integer `[0, upper_bound)` (last exclusive). `upper_bound` must be `0xffffffff` at most.

``` js
sodium.randombytes_buf(buffer)
```
Fill `buffer` with random data.

``` js
sodium.randombytes_buf_deterministic(buffer, seed)
```
Fill `buffer` with random data, generated from `seed`. `seed` must be a `buffer` of at least `sodium.randombytes_SEEDBYTES` length.
