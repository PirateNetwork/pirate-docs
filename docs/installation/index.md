---
layout: default
title: Installation
nav_order: 2
---

## Pirate Installation Guide

### Build from source (recommended)

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

See instructions for Ubuntu / Debian here.
See instructions for Centos 7 / 8 here.
