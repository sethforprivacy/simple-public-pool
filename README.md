# simple-public-pool
A repository to simplify spinning up a new Public Pool instance via Docker Compose.

For the full steps required to setup a public-pool instance using this repo, please read the full guide on my blog:

https://sethforprivacy.com/guides/run-your-own-bitcoin-pool

## Troubleshooting

Common issues when first bringing the stack up (see [#2](https://github.com/sethforprivacy/simple-public-pool/issues/2)):

- **`Could not reach RPC host` / `connect ECONNREFUSED ...:8332`** — the pool reaches bitcoind over the internal Compose network by service name, not via a LAN IP. Keep `BITCOIN_RPC_URL=http://bitcoind` and `BITCOIN_RPC_PORT=8332` as in `pool.env.example`. Port 8332 does not need to be published; only P2P port 8333 is exposed on the host.
- **`ZMQ Unable to connect, Retrying`** — ZeroMQ endpoints require the `tcp://` scheme in both places: `zmqpubrawblock=tcp://0.0.0.0:3000` in `bitcoin.conf` and `BITCOIN_ZMQ_HOST="tcp://bitcoind:3000"` in `pool.env`.
- **Connection drops after a Docker restart** — Compose DNS names (`bitcoind`) stay stable across restarts; hardcoded LAN/IP addresses do not. Prefer the service names throughout `pool.env`.

## Credit

Please note that **all** credit here belongs to the fantastic creator of [public-pool](https://github.com/benjamin-wilson/public-pool), [Benjamin Wilson](https://github.com/benjamin-wilson). If you appreciate the ability to easily spin up your own pool in a few minutes with 100% FOSS code, please consider taking a moment and sending him a donation for his time! You can find his donation details on the bottom of his pool instance (and any other pools that are created this way):

- <https://web.public-pool.io/#/>
