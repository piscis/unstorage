# CloudFlare KV (binding)

Store data in [Cloudflare KV](https://developers.cloudflare.com/workers/runtime-apis/kv) and access from worker bindings.

**Note:** This driver only works in a cloudflare worker environment, use [`cloudflare-kv-http`](/drivers/cloudflare-kv-http) for other environments.

You need to create and assign a KV. See [KV Bindings](https://developers.cloudflare.com/workers/runtime-apis/kv#kv-bindings) for more information.

```js
import { createStorage } from "unstorage";
import cloudflareKVBindingDriver from "unstorage/drivers/cloudflare-kv-binding";

// Using binding name to be picked from globalThis
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: "STORAGE" }),
});

// Directly setting binding
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: globalThis.STORAGE }),
});

// Using from Durable Objects and Workers using Modules Syntax
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: this.env.STORAGE }),
});

// Using binding name to be picked from globalThis and configure TTL to expire items in 60 seconds
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: "STORAGE", {ttl: { expirationTtl: 60 } }),
});

// Using outside of Cloudflare Workers (like Node.js)
// Use cloudflare-kv-http
```

**Options:**

- `binding`: KV binding or name of namespace. Default is `STORAGE`.
- `base`: Adds prefix to all stored keys
- `ttl`: Configures global TTL settings
  - `expiration`: TTL for item since epoch
  - `expirationTtl`: TTL for all items in seconds

**Transaction options:**

- ttl: Supported for setItem(key, value, { ttl: number /* seconds */ })
- `expiration`: TTL for item since epoch. Example: `setItem(key, value, { expiration: number /* seconds since epoch */ })`
- `expirationTtl`: TTL for all items in seconds. Example: `setItem(key, value, { expirationTtl: number /* TTL in seconds | min value 60 */ })`
