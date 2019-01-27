# Isomorphic-Rpc
Exposes a Javascript object that maps all methods to RPC calls using `Promises`"

## Web3.js Has Some Issues

- Mostly it's *huge*
- It doesnt follow the spec (i.e. spec has `eth_getBlockByHash`, web3 has only `getBlock`)
- It randomly prints some values as hex `string` ("0x1234") while others com back as `number` and for some odd reason `block.difficulty` is returned as a `string` of decimal digits (lol WAHT?)
- New RPC methods like `getProof` are not supported (in web3 or other libraries) until each piece of software in the chain publishes a feature update to support it.

## Solution

Using Ecmascript's new `Proxy` functionality you can create an object such that when you call a missing method on it, it instead calls a "handler" function that has access to the method name called.

The library uses this to automatically create an RPC request from any method name given. So any brand new or _experimental_ RPC method your client software supports will be exposed.

## Extremely Lightweight
The module is 37 lines of code. There is only 1 dependency (which branches into 3 small packages). It enables the library to be:

## Isomorphic
The use of this library should work identically in both Node and all desktop/mobile Browsers (except Internet Explorer)

### Also
This should probably work on *all* Javascript RPC applications because of its agnosticism to the actual methods being called.

But I haven't tried that yet so let me know!
