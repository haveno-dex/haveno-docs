# Basic Tor Setup

By default, Haveno manages a dynamic onion service automatically.

Basic configuration is supported if using the default Tor setup, or if connecting via a control port (e.g. `--torControlPort=9051`).

To use more advanced features in `torrc`, or if your node is experiencing a sustained Denial of Service (DoS) attack, connect to a [hardened Tor](./hardened-tor.md) instance instead.

## Basic configuration

### `--hiddenServiceParams`
**Default:** `PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100`  
Passes configuration directly to the `ADD_ONION` dynamic service to enable and tune Proof of Work defense.

### `--hiddenServiceFlags`
**Default:** `""`  
Passes behavior modifiers directly to the ADD_ONION command. These are binary or state-based toggles rather than key-value pairs.

### `--torrcOptions`
**Default:** `NumCPUs 0` (use all available CPUs)   
A list of entries to amend to Haveno's internal `torrc`. Note that critical system entries cannot be overwritten.  
*Format: Option Value[, ...]*

### `--torrcFile`
**Default:** `""`  
Path to an existing `torrc` file whose entries are added to Haveno's internal `torrc`. Note that critical system entries cannot be overwritten.  
*Format: /path/to/torrc*

## Example configuration

```
--hiddenServiceParams="PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100" \
--hiddenServiceFlags="" \
--torrcOptions="NumCPUs 0" \
```

## Connect through a proxy

If your network only allows outbound traffic through a proxy, route the included Tor through it with `--torrcOptions`:

```
--torrcOptions="Socks5Proxy host:port" \
```

Use `HTTPSProxy host:port` instead for an HTTPS (CONNECT) proxy, common on corporate networks. Add credentials if required, e.g. `Socks5ProxyUsername user, Socks5ProxyPassword pass` or `HTTPSProxyAuthenticator user:pass`.

!!! Info "Note"
    A proxy helps Tor reach the network through a restrictive firewall. If Tor itself is blocked, use [WebTunnels](./webtunnels.md) or another bridge instead.

## Enable Tor logging

By default, Tor's own logs are not written to a file, which can make connection problems hard to diagnose. Capture them with a `Log` entry via `--torrcOptions`:

```
--torrcOptions="Log notice file tor.log" \
```

A relative filename is saved inside Haveno's Tor directory; use an absolute path to write elsewhere.

Use the `notice` level for troubleshooting; more verbose levels (`info`, `debug`) may record sensitive information.
