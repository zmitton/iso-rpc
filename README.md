# Isomorphic-Rpc
Exposes a Javascript object that maps all methods to RPC calls using `Promises`"

## For Ethereum Applications
In general this should support any RPC application, but I made it to simplify my ethereum applications and I've only used/tested it on the same.

## Web3.js Has Some Issues
- Mostly it's *huge*
- It doesn't follow the spec (i.e. spec has `eth_getBlockByHash`, web3 has only `getBlock`)
- It randomly prints some values as hex `string` ("0x1234") while others come back as `number` and for some odd reason `block.difficulty` is returned as a `string` of decimal digits (lol WAHT?)
- New RPC methods like `getProof` are not supported (in web3 or other libraries) until each piece of software in the chain publishes a feature update to support it.

## Solution
Using Ecmascript's new `Proxy` functionality you can create an object such that when you call a missing method on it, it instead calls a "handler" function that has access to the method name called.

The library uses this to automatically create an RPC request from any method name given. So any brand new or _experimental_ RPC method your client software supports will be exposed.

## Extremely Lightweight
The module is 37 lines of code. There is only 1 dependency (which branches into 3 small packages). It's only there to enables the library to be isomorphic.

## Isomorphism
The use of this library should work identically in both Node and all desktop/mobile Browsers (except Internet Explorer because bill gates is busy curing malaria)

### Also
This should probably work on *all* Javascript RPC applications because of its agnosticism to the actual methods being called.

But I haven't tried that yet so let me know!

Seems to be a good pattern because refactoring my code to use isomorphic-rpc is making tons of lines disappear.

## Use
```
npm install iso-rpc
```
```javascript
const Rpc = require('./iso-rpc')
let rpc = new Rpc('http://localhost:8545') //for local node (default)
// or  "https://mainnet.infura.io" or even "https://web3.gastracker.io" etc...

rpc.web3_sha3("0x").then((hash)=>{ console.log(hash) }).catch((e)=>{console.log(e)})
```
or within an `async` function:
```javascript
let hash = await rpc.web3_sha3("0x")
```

## Docs
The [JSON-RPC Page](https://github.com/ethereum/wiki/wiki/JSON-RPC) is the most up to date list of supported Ethereum RPC calls.

Also the [RPC 2.0 Specification](https://www.jsonrpc.org/specification) could be useful in updating this package. If a PR will help it better supports the RPC spec, I'll merge it in.



