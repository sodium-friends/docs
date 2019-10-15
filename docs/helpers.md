---
id: helpers
title: Helpers
sidebar_label: Helpers
---

Bindings to various helper functions. [See the libsodium helpers docs for more information](https://download.libsodium.org/doc/helpers/).

``` js
var bool = sodium.sodium_memcmp(b1, b2)
```
Compare `b1` with `b2`, in **constant-time** for `b1.length`.
* `b1` must be `buffer`
* `b2` must be `buffer` and must be `b1.length` bytes

Returns `true` when equal, otherwise `false`.

``` js
var direction = sodium.sodium_compare(b1, b2)
```
Compare `b1` with `b2`, regarding either as little-endian encoded number.
* `b1` must be `buffer`
* `b2` must be `buffer` and must be `b1.length` bytes

Returns `1`, `0`, or `-1` on whether `b1`is greater, equal, or less than `b2`. This is the same scheme as `Array.prototype.sort` expect.

``` js
sodium.sodium_add(a, b)
```
Adds `b` to `a` (wrapping), regarding either as little-endian encoded number, and writing the result into `a`.
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_sub(a, b)
```
Subtracts `b` from `a` (wrapping), regarding either as little-endian encoded number, and writing the result into `a`.
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_increment(buf)
```
Increments `buf` as a little-endian number. This operation is **constant-time** for the length of `buf`.
* `buf` must be `buffer`

``` js
var bool = sodium.sodium_is_zero(buf, len)
```
Tests whether `buf` is all zero for `len` bytes. This operation is **constant-time** for `len`.
* `len` must be an integer at most `buf.length`

Returns `true` if all `len` bytes are zero, otherwise `false`.
