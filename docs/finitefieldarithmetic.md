---
id: finitefieldarithmetic
title: Finite field arithmetic
sidebar_label: Finite field arithmetic
---

Bindings for the crypto_scalarmult_ed25519 and crypto_core_ed25519 API. [See the libsodium docs for more information](https://download.libsodium.org/doc/advanced/point-arithmetic).

### Constants
* `crypto_scalarmult_ed25519_BYTES`
* `crypto_scalarmult_ed25519_SCALARBYTES`
* `crypto_core_ed25519_BYTES`
* `crypto_core_ed25519_UNIFORMBYTES`
* `crypto_core_ed25519_SCALARBYTES`
* `crypto_core_ed25519_NONREDUCEDSCALARBYTES`

``` js
var bool = crypto_core_ed25519_is_valid_point(p)
```
>The crypto_core_ed25519_is_valid_point() function checks that p represents a point on the edwards25519 curve, in canonical >form, on the main subgroup, and that the point doesn't have a small order.

* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Returns `true` or `false`

``` js
crypto_core_ed25519_from_uniform(p, r)
```
Maps a `crypto_core_ed25519_UNIFORMBYTES` bytes vector (usually the output of a hash function) to a a valid curve point and stores its compressed representation in `p`.

The point is guaranteed to be on the main subgroup.
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `r` must be a `buffer` of at least `crypto_core_ed25519_UNIFORMBYTES` bytes

``` js
crypto_scalarmult_ed25519(q, n, p)
```
Multiply point `p` by scalar `n` and store its compressed representation in `q`.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw if `n` is zero or `p` is an invalid curve point.

``` js
crypto_scalarmult_ed25519_base(q, n)
```
Multiply the basepoint by scalar `n` and store its compressed representation in `q`. Note that `n` will be clamped.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw if `n` is zero.

``` js
crypto_scalarmult_ed25519_noclamp(q, n, p)
```
Multiply point `p` by scalar `n` and store its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw if `n` is zero or `p` is an invalid curve point.

``` js
crypto_scalarmult_ed25519_base_noclamp(q, n)
```
Multiply the basepoint by scalar `n` and store its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw if `n` is zero.

``` js
crypto_core_ed25519_add(r, p, q)
```
Add point `q` to `p`, storing the result to `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Will throw if `p`, `q` are not valid curve points

``` js
crypto_core_ed25519_sub(r, p, q)
```
Subtract point `q` to `p`, storing the result to `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Will throw if `p`, `q` are not valid curve points

``` js
crypto_core_ed25519_scalar_random(r)
```
Generate random scalar in `]0..L[`, storing it in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_reduce(r, s)
```
Reduce `s mod L`, storing it in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_NONREDUCEDSCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_invert(recip, s)
```
Find `recip` such that `s * recip = 1 (mod L)`, storing it in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_negate(neg, s)
```
Find `neg` such that `s + neg = 0 (mod L)`, storing it in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_complement(comp, s)
```
Find `comp` such that `s + comp = 1 (mod L)`, storing it in `recip`.
* `comp` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_add(z, x, y)
```
Add `x` and `y` such that `x + y = z (mod L)`, storing it in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_sub(z, x, y)
```
Subtract `x` and `y` such that `x - y = z (mod L)`, storing it in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
