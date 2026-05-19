# Tor Overview

Haveno uses Tor to connect to a peer-to-peer network.

By default, Haveno connects automatically using an included version of Tor, but users can connect through an external instance of Tor instead.

If you are a [Whonix or Qubes OS user](./operating-systems.md), you must connect through an external, static Tor instance (see below).

---

## Install Tor (optional)

If you want to connect through an external Tor instance, first [install tor](https://support.torproject.org/little-t-tor/getting-started/installing/) for your system.

Alternatively you can install the [Tor Browser](https://www.torproject.org/download/) and then launch the embedded `tor` binary.

Start Tor with `tor [-f /path/to/torrc]`. Alternatively you can manage Tor as a system service with `systemctl start tor`, `systemctl reload tor`, etc.

See [instructions](./operating-systems.md) for specific operating systems.

---

## Connect using the included Tor (default)

This works out of the box by automatically launching an included version of Tor managed completely by Haveno.

This method creates a dynamic onion service in your application folder and supports [basic configuration](./basic-tor.md) only.

## Connect via control port

Users can connect to an external instance of Tor using the control port flag, e.g. `--torControlPort=9051`.

The control port is defined in your external `torrc` file. For example:

```
ControlPort 9051
SocksPort 127.0.0.1:9050
```

Haveno will use the control port to create a dynamic onion service in your application folder. This mode supports [basic configuration](./basic-tor.md) only.

!!! Info "Note"
    Do not modify the internal `torrc` file inside the /xmr_mainnet/tor/ subdirectory. That file is used internally by Haveno and will be overwritten on every application startup.

## Connect using a static Tor instance

Haveno can bind directly to a static onion service running on your system, using the startup flag `--hiddenServiceAddress=your_onion_address.onion [--nodePort=9999]`.

This mode allows advanced features in your external `torrc` file and is the required configuration for [Whonix and Qubes OS users](./operating-systems.md). See the [Hardened Tor](./hardened-tor.md) guide for extended configuration options.

!!! warning "Denial of Service Protection"
    If your node experiences sustained DDoS attacks, use this mode to connect to a [hardened Tor](./hardened-tor.md) instance.