# LRU-Memoize _(@digitalcredentials/lru-memoize)_

[![Node.js CI](https://github.com/digitalcredentials/lru-memoize/workflows/Node.js%20CI/badge.svg)](https://github.com/digitalcredentials/lru-memoize/actions?query=workflow%3A%22Node.js+CI%22)
[![NPM Version](https://img.shields.io/npm/v/@digitalcredentials/lru-memoize.svg)](https://npm.im/@digitalcredentials/lru-memoize)

> A Memoized wrapper around the javascript [`lru-cache`](https://www.npmjs.com/package/lru-cache) library.

## Table of Contents

- [Background](#background)
- [Install](#install)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)

## Background

(Forked from [`digitalbazaar/lru-memoize` v2.0.0](https://github.com/digitalbazaar/lru-memoize)
to provide TypeScript and ReactNative compatibility.)

lru-memoize is used to
to [memoize](https://en.wikipedia.org/wiki/Memoization) promises
(as opposed to just the results of the operations),
which helps in high-concurrency use cases. (And in turn, it uses
[`lru-cache`](https://www.npmjs.com/package/lru-cache) under the hood.)

## Install

To install locally (for development):

```
git clone https://github.com/digitalcredentials/lru-memoize.git
cd lru-memoize
npm install
```

## Usage

To import:

```js
import { LruCache } from '@digitalcredentials/lru-memoize'
// or
const { LruCache } = require('@digitalcredentials/lru-memoize')
```

The memoized `LruCache` constructor passes any options given to it through to
the `lru-cache` constructor, so  see that repo for the full list of cache
management options. Commonly used ones include:

* `max` (default: 100) - maximum size of the cache.
* `maxAge` (default: 5 sec/5000 ms) - maximum age of an item in ms.
* `updateAgeOnGet` (default: `false`) - When using time-expiring entries with
  `maxAge`, setting this to true will make each entry's effective time update to
  the current time whenever it is retrieved from cache, thereby extending the
  expiration date of the entry.

This library is useful for caching (in a deterministic memoized fashion) expensive or long-running functions, such as
API requests, database lookups, and so on.

For example, say you have a function `fetchStatus()` that retrieves a result from a web API (here, simulated with a 
`delay()` wait). To cache the result of this function:

```js
import { LruCache } from '@digitalcredentials/lru-memoize'

// Cache expiration/TTL: 5 seconds
const myCache = new LruCache({ maxAge: 5000 })

async function fetchStatus() {
  // simulate an async task
  await delay(100);
  executedTestFn = true;
  return {success: true, timestamp: Date.now()};
}

// Load the cached result if it's present, otherwise, perform the operation
const result = await myCache.memoize({
  key: 'myApiResults',
  fn: fetchStatus
});

// You can also memoize a particular call to a function, using anonymous arrow functions:
const url = 'https://api.example'
const result = await myCache.memoize({
  key: 'myResults',
  fn: async () => fetchMyResultsFromWeb({ url })
})
```

The `key` param is used to namespace the caches, in case the same `LruCache` instance is being used to cache different
types of operations/functions.

## Contribute

PRs accepted.

If editing the Readme, please conform to the
[standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

* MIT License - DCC - TypeScript compatibility.
* New BSD License (3-clause) © 2020-2021 Digital Bazaar - Initial implementation.
