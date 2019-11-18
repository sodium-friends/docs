---
id: finitefieldarithmetic
title: Finite Field Arithmetic
sidebar_label: Finite Field Arithmetic
---

Bindings for the crypto_scalarmult_ed25519 and crypto_core_ed25519 API. [See the libsodium docs for more information](https://download.libsodium.org/doc/advanced/point-arithmetic).

### Constants
* `crypto_scalarmult_ed25519_BYTES`
* `crypto_scalarmult_ed25519_SCALARBYTES`
* `crypto_core_ed25519_BYTES`
* `crypto_core_ed25519_UNIFORMBYTES`
* `crypto_core_ed25519_SCALARBYTES`
* `crypto_core_ed25519_NONREDUCEDSCALARBYTES`

***
## `crypto_core_ed25519_is_valid_point`
![sodium-native][node]
``` js
var bool = sodium.crypto_core_ed25519_is_valid_point(p)
```
>The crypto_core_ed25519_is_valid_point() function checks that p represents a point on the edwards25519 curve, in canonical >form, on the main subgroup, and that the point doesn't have a small order.

* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Returns `true` or `false`.
***
## `crypto_core_ed25519_from_uniform`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_from_uniform(p, r)
```
Maps a `crypto_core_ed25519_UNIFORMBYTES` bytes vector (usually the output of a hash function) to a a valid curve point and stores its compressed representation in `p`.

The point is guaranteed to be on the main subgroup.
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `r` must be a `buffer` of at least `crypto_core_ed25519_UNIFORMBYTES` bytes

***
## `crypto_scalarmult_ed25519`
![sodium-native][node]
``` js
sodium.crypto_scalarmult_ed25519(q, n, p)
```
Multiplies point `p` by scalar `n` and stores its compressed representation in `q`.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw, if `n` is zero or `p` is an invalid curve point.
***
## `crypto_scalarmult_ed25519_base`
![sodium-native][node]
``` js
sodium.crypto_scalarmult_ed25519_base(q, n)
```
Multiplies the base point by scalar `n` and stores its compressed representation in `q`. Note that `n` will be clamped.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw if `n` is zero.
***
## `crypto_scalarmult_ed25519_noclamp`
![sodium-native][node]
``` js
sodium.crypto_scalarmult_ed25519_noclamp(q, n, p)
```
Multiplies point `p` by scalar `n` and stores its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw, if `n` is zero or `p` is an invalid curve point.
***
## `crypto_scalarmult_ed25519_base_noclamp`
![sodium-native][node]
``` js
sodium.crypto_scalarmult_ed25519_base_noclamp(q, n)
```
Multiplies the base point by scalar `n` and stores its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw, if `n` is zero.
***
## `crypto_core_ed25519_add`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_add(r, p, q)
```
Adds point `q` to `p` and stores the result in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Note this function will throw, if `p`, `q` are not valid curve points
***
## `crypto_core_ed25519_sub`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_sub(r, p, q)
```
Subtracts point `q` to `p` and stores the result in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Note this function will throw, if `p`, `q` are not valid curve points.
***
## `crypto_core_ed25519_scalar_random`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_random(r)
```
Generates random scalar in `]0..L[` and stores the result in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_reduce`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_reduce(r, s)
```
Reduces `s mod L` and stores the result in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_NONREDUCEDSCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_invert`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_invert(recip, s)
```
Finds `recip` such that `s * recip = 1 (mod L)` and stores the result in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_negate`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_negate(neg, s)
```
Finds `neg` such that `s + neg = 0 (mod L)` and stores the result in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_complement`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_complement(comp, s)
```
Finds `comp` such that `s + comp = 1 (mod L)` and stores the result in `recip`.
* `comp` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_add`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_add(z, x, y)
```
Adds `x` and `y` such that `x + y = z (mod L)` and stores the result in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

***
## `crypto_core_ed25519_scalar_sub`
![sodium-native][node]
``` js
sodium.crypto_core_ed25519_scalar_sub(z, x, y)
```
Subtracts `x` and `y` such that `x - y = z (mod L)` and stores the result in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
