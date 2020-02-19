---
id: memoryprotection
title: Memory Protection
sidebar_label: Memory Protection
---

Bindings for the secure memory API. [See the libsodium "Securing memory allocations" docs for more information](https://download.libsodium.org/doc/memory_management).
***
## `sodium_memzero` 
![sodium-native][node]
``` js
sodium.sodium_memzero(buf)
```
Zeros out the data in `buf`.
***
## `sodium_mlock`
![sodium-native][node]
``` js
sodium.sodium_mlock(buf)
```
Locks the memory contained in `buf`.
***
## `sodium_munlock`
![sodium-native][node]
``` js
sodium.sodium_munlock(buf)
```
Unlocks previously `sodium_mlock`'ed memory contained in `buf`. This will also `sodium_memzero` of `buf`.
***
## `sodium_malloc`
![sodium-native][node]
``` js
var buffer = sodium.sodium_malloc(size)
```
Allocates a `buffer` of `size` which is memory protected. See [libsodium docs](https://download.libsodium.org/doc/memory_management#guarded-heap-allocations) for details. Be aware that many `buffer`-methods may break the security guarantees of `sodium_malloc`'ed memory. To check if a `buffer` is a "secure" `buffer`, you can call access the getter `buffer.secure` which will be `true`.
***
## `sodium_mprotect_noaccess`
![sodium-native][node]
``` js
sodium.sodium_mprotect_noaccess(buf)
```
Makes `buf` which was allocated using `sodium_malloc` inaccessible, crashing the process if any access is attempted. Note that this will have no effect for normal `buffer`'s.
***
## `sodium_mprotect_readonly`
![sodium-native][node]
``` js
sodium.sodium_mprotect_readonly(buf)
```
Makes `buf` which was allocated using `sodium_malloc` read-only, crashing the process if any writing is attempted. Note that this will have no effect for normal `buffer`'s.
***
## `sodium_mprotect_readwrite`
![sodium-native][node]
``` js
sodium.sodium_mprotect_readwrite(buf)
```
Makes `buf` which was allocated using `sodium_malloc` read-write, undoing `sodium_mprotect_noaccess` or `sodium_mprotect_readonly`. Note that this will have no effect for normal `buffer`'s.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
