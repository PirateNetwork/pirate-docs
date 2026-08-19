---
layout: default
title: Hidden Services (Tor & I2P)
parent: Guides
nav_order: 8
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# Running pirated Behind Your Own Tor & I2P
{: .no_toc }

By default, `pirated` can launch and manage its own bundled Tor and i2pd daemons (`torautostart`/`i2pdautostart`, both on by default) so Tor/I2P support works out of the box with zero setup.

This guide is for the opposite case: you already run Tor and/or i2pd yourself - as a system service, shared with other applications, under your own package/version, monitoring, etc. - and just want `pirated` to use that instance instead of launching its own. Everything else about how `pirated` behaves on Tor/I2P (stream isolation, private tx relay, onion service creation, I2P identity rotation) is left at its normal defaults; this guide only changes who's responsible for the Tor/i2pd process itself.

## Prerequisites

- A synced `pirated` node - see [Installation](../../installation).
- Tor and/or i2pd installed via your distro's package manager (or however you already manage them) and *not* the embedded daemon `pirated` would otherwise launch.

## Tor

### 1. Install Tor

```
sudo apt-get install tor
```

### 2. Enable the control port

Edit `/etc/tor/torrc` and add:

```
ControlPort 9051
CookieAuthentication 1
CookieAuthFileGroupReadable 1
```

Leave `SOCKSPort` alone - the Debian/Ubuntu Tor package already listens on `127.0.0.1:9050` by default, which matches `pirated`'s own `-torsocksport` default.

Restart Tor:

```
sudo systemctl restart tor
```

### 3. Let pirated authenticate to the control port

`pirated` prefers Tor's SAFECOOKIE authentication - it reads the auth cookie file Tor writes under its data directory (typically `/var/run/tor/control.authcookie` on a Debian/Ubuntu package install; check your distro if it's elsewhere). The simplest way to grant access is to add the user `pirated` runs as to Tor's group (`debian-tor` on Debian/Ubuntu):

```
sudo usermod -a -G debian-tor $USER
```

Log out and back in (or start a new shell) for the group change to take effect.

Note
{: .label .label-blue }
If you'd rather not use group-based cookie access, set a control-port password instead: `tor --hash-password yourpassword` and add the printed `HashedControlPassword 16:...` line to `torrc`, then set `torpassword=yourpassword` in PIRATE.conf (the plaintext password, not the hash).

### 4. PIRATE.conf

```
torautostart=0
listenonion=1
```

`torcontrol` (default `127.0.0.1:9051`) and `torsocksport` (default `127.0.0.1:9050`) already match the stock Tor package's ports, so there's nothing else to set unless you moved them. Once `pirated` authenticates to the control port, it automatically uses `torsocksport` as the SOCKS proxy for onion peers, generates a persistent ed25519 onion service key on first run (cached under the datadir's `onion_v3_private_key`), and starts advertising it.

### 5. Verify

```
tail -f ~/.komodo/PIRATE/debug.log | grep -i tor
```

You should see a successful control-port authentication and a generated `.onion` address. `pirate-cli getnetworkinfo` will also list the onion address under `localaddresses` once the hidden service is up.

## I2P

### 1. Install i2pd

```
sudo apt-get install i2pd
```

### 2. Enable the SAM API

Edit `/etc/i2pd/i2pd.conf` and add (or uncomment) under the `[sam]` section:

```
[sam]
enabled = true
address = 127.0.0.1
port = 7656
```

Restart i2pd:

```
sudo systemctl restart i2pd
```

### 3. PIRATE.conf

```
i2pdautostart=0
i2psam=127.0.0.1:7656
```

Note
{: .label .label-red }
Unlike Tor, `i2psam` has no default once `i2pdautostart` is disabled - if you leave it unset, I2P stays off entirely. It must point at your i2pd's SAM address explicitly.

`i2pacceptincoming` already defaults to 1, so inbound I2P connections are accepted automatically as soon as `i2psam` is set - no extra flag needed unless you specifically want outbound-only I2P (`i2pacceptincoming=0`).

### 4. Verify

```
tail -f ~/.komodo/PIRATE/debug.log | grep -i i2p
```

## Combining with lightwalletd (optional)

If you're also running lightwalletd (see the [Lite Daemon guide](lite_daemon)), it can reuse this same standalone Tor/i2pd setup with `--tor-enable`/`--i2p-enable` - it reads `-torcontrol`/`-torpassword`/`-i2psam` straight out of the same PIRATE.conf you just configured, so there's nothing further to set up. See [Lightwalletd Parameters](lited_params) for details.

## What this doesn't change

Everything else about `pirated`'s Tor/I2P behavior is untouched and stays at its normal default: stream isolation (`proxyrandomize=1`), restricting locally-originated transaction relay to Tor/I2P peers when available (`privatetxrelay=1`, falling back to clearnet peers if none are connected via `privatetxrelayfallback=1`), and the rotating pool of short-lived I2P relay identities (`i2pidentityrotation=1`). This guide only swaps which process manages Tor/i2pd - it doesn't turn any of that off.
