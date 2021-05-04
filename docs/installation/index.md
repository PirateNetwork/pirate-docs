---
layout: default
title: Installation
nav_order: 2
has_children: true
permalink: docs/installation
---

## Pirate Installation Guide
{: .no_toc }
### Build from source (recommended)
{: .no_toc }
Please see below the instructions to install Pirate daemon, best practices and tips.

```diff
- !!! IMPORTANT !!!
```
PirateChain (ARRR) is a fully shielded chain and ***ONLY*** uses zs addresses - transparent addresses are ***NOT*** permitted to perform transactions other than generating coinbase (mining) and notarizations by elected KMD notaries; the usual RPC commands generally don't apply

RPC commands for Pirate start with z_ -> see **pirate-cli help** for a list of available commands

Tested and supported on:
* Ubuntu:
  * 18.04
  * 20.04
* Debian:
  * 9
  * 10
* CentOS:
  * 7
  * 8
