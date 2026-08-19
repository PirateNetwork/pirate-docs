---
layout: default
title: Lightwalletd Parameters
parent: Lite Daemon
grand_parent: Guides
nav_order: 1
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Lightwalletd parameters
{: .no_toc}

Run `lightwalletd --help` for the authoritative, in-binary list. Every flag below can also be set in a `lightwalletd.yml` config file (see [Configuration file](#configuration-file--environment-variables)) or via an environment variable.

Note
{: .label .label-red }
The old single-dash flags from earlier versions (`-bind-addr`, `-conf-file`, `-cache-size`, `-no-tls`) no longer exist. The binary was rewritten on Cobra/Viper and uses double-dash flags with new names, listed below.

### Network

#### --grpc-bind-addr string
```
the address to listen for grpc on (default "127.0.0.1:9067")
```

#### --http-bind-addr string
```
the address to listen for http on (default "127.0.0.1:9068")
```
Serves `/metrics` (Prometheus) and `/params/` (Sapling parameter file downloads). Keep this bound to loopback unless you intentionally want to expose either endpoint.

#### --grpc-logging-insecure
```
enable grpc logging to stderr (default false)
```

### TLS

#### --tls-cert string
```
the path to a TLS certificate (default "./cert.pem")
```

#### --tls-key string
```
the path to a TLS key file (default "./cert.key")
```

#### --no-tls-very-insecure
```
run without the required TLS certificate, only for debugging, DO NOT use in production (default false)
```
Use this when a reverse proxy (e.g. NGINX) is terminating TLS in front of lightwalletd.

#### --gen-cert-very-insecure
```
run with self-signed TLS certificate, only for debugging, DO NOT use in production (default false)
```

### Pirate node connection

#### --pirate-conf-path string
```
conf file to pull RPC creds from (default "./PIRATE.conf")
```
Reads `rpcuser`/`rpcpassword`/`rpcbind`/`rpcport` out of the target PIRATE.conf. Superseded per-field by `--rpcuser`/`--rpcpassword`/`--rpchost`/`--rpcport` below if those are set instead.

#### --rpcuser / --rpcpassword / --rpchost / --rpcport string
```
RPC user name / RPC password / RPC host / RPC host port
```
Connect directly without reading PIRATE.conf. All four must be set together, otherwise lightwalletd falls back to `--pirate-conf-path`.

### Data & sync

#### --data-dir string
```
data directory (such as db) (default "/var/lib/lightwalletd")
```
Replaces the old in-memory `-cache-size` flag - compact blocks are now cached on disk under `<data-dir>/db/<chain-name>` instead of held in a fixed-size in-memory ring buffer.

#### --redownload
```
re-fetch all blocks from pirated; reinitialize local cache files (default false)
```

#### --sync-from-height int
```
re-fetch blocks from pirated start at this height (default -1)
```

### Logging

#### --log-file string
```
log file to write to (default "./server.log")
```

#### --log-level int
```
log level (logrus 1-7) (default 4)
```

### Tor & I2P

If TreasureChest (`pirated`) is running alongside lightwalletd, it already manages its own Tor and I2P daemons. These flags let lightwalletd reuse those daemons to publish itself as a hidden service / I2P destination, rather than launching its own.

#### --tor-enable
```
publish the gRPC and HTTP ports as a Tor hidden service, using the Tor daemon TreasureChest already runs (default false)
```

#### --tor-control-addr string
```
Tor control port address (default "127.0.0.1:9051")
```
Matches TreasureChest's `-torcontrol` default. Overridden by PIRATE.conf's `-torcontrol`, if set there and this flag wasn't explicitly passed.

#### --tor-password string
```
Tor control port password
```
Matches TreasureChest's `-torpassword`. Overridden by PIRATE.conf's `-torpassword`, if set there and this flag wasn't explicitly passed.

#### --tor-keys-file string
```
path to persist the onion service's private key (default "<data-dir>/tor/onion_private_key")
```

#### --i2p-enable
```
publish the gRPC port as an I2P destination, using the i2pd daemon TreasureChest already runs (default false)
```

#### --i2p-sam-addr string
```
I2P SAM API address (default "127.0.0.1:7656")
```
Matches TreasureChest's `-i2psam` default. Overridden by PIRATE.conf's `-i2psam`, if set there and this flag wasn't explicitly passed.

#### --i2p-keys-file string
```
path to persist the I2P destination's private keys (default "<data-dir>/i2p/keys.dat")
```

### Testing / development

#### --ping-very-insecure
```
allow Ping GRPC for testing (default false)
```

#### --darkside-very-insecure
```
run with GRPC-controllable mock pirated for integration testing, shuts down after --darkside-timeout minutes (default false)
```

#### --darkside-timeout int
```
override 30 minute default darkside timeout (default 30)
```

### Configuration file & environment variables

#### --config string
```
config file (default is current directory, lightwalletd.yaml)
```
Every flag above has a matching key in this YAML file (flag name with the leading `--` dropped, e.g. `grpc-bind-addr`, `pirate-conf-path`). See the shipped `lightwalletd-example.yml` for a template.

Every key can also be set through an environment variable: uppercase the key and replace `-` with `_` (e.g. `GRPC_BIND_ADDR`, `PIRATE_CONF_PATH`).

Precedence, highest to lowest: command-line flag > environment variable > config file value > built-in default.
